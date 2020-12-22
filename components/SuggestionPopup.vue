<template>
  <div class="suggestion-popup">
    <slot>
      It is popup
    </slot>
  </div>
</template>

<script>
export default {
  name: "SuggestionPopup"
};
</script>

<style>
.suggestion-popup {
  border: solid 1px darkgrey;
  background-color: darkgray;
  border-radius: 4px;
  padding: 2px;
  width: fit-content;
}
</style>
