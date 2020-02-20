<template>
  <div class="custom-select" :tabIndex="tabIndex" @blur="open = false">
    <div class="selected" :class="{ open: open }" @click="open = !open">
      {{ selected }}
    </div>
    <div class="items" :class="{ selectHide: !open }">
      <div
        class="item"
        v-for="(option, i) of options"
        :key="i"
        @click="
          selected = option
          open = false
          $emit('filterChanged', option.toLowerCase())
        "
      >
        {{ option }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    options: {
      type: Array,
      required: true
    },
    tabIndex: {
      type: Number,
      required: false,
      default: 0
    }
  },
  data() {
    return {
      selected: this.$route.query.species || this.options[0],
      open: false
    }
  }
}
</script>

<style lang="scss">
.custom-select {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: left;
  outline: none;
  line-height: 40px;
  background-color: white;
  border-radius: 6px;
  border: none;
}

.selected {
  padding-left: 8px;
  cursor: pointer;
  user-select: none;
  height: 100%;
  display: flex;
  align-items: center;

  &:active,
  &:focus,
  &:focus-within {
    outline: 0;
    box-shadow: none;
  }
}

.selected.open {
  border-radius: 6px 6px 0px 0px;
}

.selected:after {
  position: absolute;
  content: '';
  right: 10px;
  width: 0;
  height: 0;
  border: 4px solid black;
  border-color: #000 transparent transparent transparent;
}

.items {
  border-radius: 0px 0px 6px 6px;
  overflow: hidden;
  position: absolute;
  left: 0;
  right: 0;
  background-color: white;
  z-index: 999;
}

.item {
  padding-left: 8px;
  cursor: pointer;
  user-select: none;
}

.item:hover {
  background-color: #92bca6;
}

.selectHide {
  display: none;
}
</style>
