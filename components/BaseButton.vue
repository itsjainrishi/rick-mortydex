<template>
  <button class="base-button">
    <slot></slot>
  </button>
</template>

<script>
export default {}
</script>

<style lang="scss">
.base-button {
  background: white;
  border: 1px solid darkseagreen;
  padding: 10px;
  margin: 10px;
  cursor: pointer;
}
</style>
