<template>
  <button class="base-pagination-trigger" @click="onClick">
    {{ pageNumber }}
  </button>
</template>
<script>
export default {
  props: {
    pageNumber: {
      type: Number,
      required: true
    }
  },
  methods: {
    onClick() {
      this.$emit('loadPage', this.pageNumber)
    }
  }
}
</script>

<style lang="scss">
.base-pagination-trigger {
  background: white;
  border: 1px solid darkseagreen;
  padding: 10px;
  margin: 10px;
  cursor: pointer;
}
</style>
