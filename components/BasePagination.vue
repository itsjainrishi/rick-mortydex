<template>
  <div class="base-pagination">
    <base-button
      :disabled="isPreviousButtonDisabled"
      @click.native="previousPage"
    >
      ←
    </base-button>
    <base-pagination-trigger
      v-for="paginationTrigger in paginationTriggers"
      :key="paginationTrigger"
      :class="{
        'base-pagination__description--current':
          paginationTrigger === currentPage
      }"
      class="base-pagination__description"
      :page-number="paginationTrigger"
      @loadPage="onLoadPage"
    />
    <base-button :disabled="isNextButtonDisabled" @click.native="nextPage">
      →
    </base-button>
  </div>
</template>

<script>
import BaseButton from './BaseButton.vue'
import BasePaginationTrigger from './BasePaginationTrigger.vue'

export default {
  components: {
    BaseButton,
    BasePaginationTrigger
  },
  props: {
    currentPage: {
      type: Number,
      required: true
    },
    pageCount: {
      type: Number,
      required: true
    },
    visiblePagesCount: {
      type: Number,
      default: 5
    }
  },
  computed: {
    isPreviousButtonDisabled() {
      return this.currentPage === 1
    },
    isNextButtonDisabled() {
      return this.currentPage === this.pageCount
    },
    paginationTriggers() {
      const currentPage = this.currentPage
      const pageCount = this.pageCount
      const visiblePagesCount = this.visiblePagesCount
      const visiblePagesThreshold = (visiblePagesCount - 1) / 2
      const pagintationTriggersArray = Array(this.visiblePagesCount - 1).fill(0)

      if (currentPage <= visiblePagesThreshold + 1) {
        pagintationTriggersArray[0] = 1
        const pagintationTriggers = pagintationTriggersArray.map(
          (paginationTrigger, index) => {
            return pagintationTriggersArray[0] + index
          }
        )
        pagintationTriggers.push(pageCount)
        return pagintationTriggers
      }

      if (currentPage >= pageCount - visiblePagesThreshold + 1) {
        const pagintationTriggers = pagintationTriggersArray.map(
          (paginationTrigger, index) => {
            return pageCount - index
          }
        )
        pagintationTriggers.reverse().unshift(1)
        return pagintationTriggers
      }

      pagintationTriggersArray[0] = currentPage - visiblePagesThreshold + 1
      const pagintationTriggers = pagintationTriggersArray.map(
        (paginationTrigger, index) => {
          return pagintationTriggersArray[0] + index
        }
      )
      pagintationTriggers.unshift(1)
      pagintationTriggers[pagintationTriggers.length - 1] = pageCount
      return pagintationTriggers
    }
  },
  methods: {
    nextPage() {
      this.$emit('nextPage')
    },
    previousPage() {
      this.$emit('previousPage')
    },
    onLoadPage(value) {
      this.$emit('loadPage', value)
    }
  }
}
</script>
