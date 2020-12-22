<template>
  <div class="editor">
    <TuiEditor
      ref="editor"
      v-model="value"
      :options="options"
      initialEditType="wysiwyg"
    ></TuiEditor>

    <div id="suggestion-popup">
      Cin chào
    </div>
  </div>
</template>

<script>
import "codemirror/lib/codemirror.css";
import "@toast-ui/editor/dist/toastui-editor.css";

import { Editor } from "@toast-ui/vue-editor";
import { EditorOptions, EditorHookMap } from "@toast-ui/editor";
import Vue from "vue";
import SuggestionPopup from "./SuggestionPopup";
import { createPopper } from "@popperjs/core";

const SuggestionPopupClass = Vue.extend(SuggestionPopup);

export default {
  components: {
    TuiEditor: Editor
  },

  props: {
    initValue: {}
  },

  data() {
    /** @type {EditorOptions} */
    const options = {
      initialValue: this.$options.props.initValue,

      hideModeSwitch: true
    };

    /** @type {EditorHookMap} */
    const hookMap = {};

    return {
      options,
      value: "",
      editor: null,
      wwEditor: null,
      editorBody: null
    };
  },

  mounted() {
    this.init();
    window.editor = this.editor;
    this.editorBody.addEventListener("keydown", this.hanldeKeydown);
  },

  methods: {
    init() {
      this.editor = this.$refs.editor.editor;
      this.wwEditor = this.editor.wwEditor;
      this.editorBody = this.wwEditor.getBody();
    },

    hanldeKeydown(event) {
      if (event.code === "Backspace") {
        const range = this.wwEditor.getRange();
        const el = range.commonAncestorContainer;
        if (el && el.querySelector("img")) {
          this.editorBody.removeChild(el);
        }
        return;
      }

      if (event.key === "@") {
        /** @type {Range} */
        const range = this.wwEditor.getRange();
        const el =
          range.commonAncestorContainer.nodeName === "DIV"
            ? range.commonAncestorContainer
            : range.commonAncestorContainer.parentElement;

        this.wwEditor.addWidget(
          range,
          document.querySelector("#suggestion-popup")
        );

        // if (el) {
        //   const vSuggestionPopup = new SuggestionPopupClass();
        //   vSuggestionPopup.$mount();
        //   const instance = createPopper(
        //     el,
        //     document.querySelector("#suggestion-popup"),
        //     {
        //       placement: "right"
        //     }
        //   );

        //   console.log(instance);
        // }
        return;
      }
    }
  }
};
</script>

<style lang="scss">
.editor {
  .tui-editor-contents {
    // img {
    //   cursor: pointer;
    // }
  }
}
</style>
