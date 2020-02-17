<template>
  <div class="page-container">
    <div class="inner">
      <div class="container">
        <section class="main">
          <div class="columns is-multiline">
            <character-component
              v-for="character in results"
              :key="character.id"
              :item="character"
              class="column is-4"
            />
          </div>
          <base-pagination
            :current-page="currentPage"
            :page-count="pageCount"
            class="characters-list__pagination"
            @nextPage="pageChangeHandle('next')"
            @previousPage="pageChangeHandle('previous')"
            @loadPage="pageChangeHandle"
          />
        </section>
      </div>
    </div>
  </div>
</template>

<script>
import axios from '~/plugins/axios'
import CharacterComponent from '@/components/Character.vue'
import BasePagination from '@/components/BasePagination.vue'

export default {
  watchQuery: ['page'],
  components: {
    CharacterComponent,
    BasePagination
  },
  async asyncData(context) {
    const {
      data: { info, results }
    } = await axios.get('/character/?page=' + context.route.query.page || 1)
    return { info, results }
  },
  data() {
    return {
      currentPage: Number(this.$route.query.page) || 1
    }
  },
  computed: {
    pageCount() {
      return this.info.pages
    }
  },
  methods: {
    pageChangeHandle(value) {
      switch (value) {
        case 'next':
          this.currentPage += 1
          break
        case 'previous':
          this.currentPage -= 1
          break
        default:
          this.currentPage = value
      }
      this.$router.push({ name: 'index', query: { page: this.currentPage } })
    }
  }
}
</script>

<style lang="scss">
.page-container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  background: #fceeb5;
  background-size: cover;
}

.inner {
  min-height: 100vh;
  width: 100%;
}

.main {
  width: 100%;
  padding: 150px 0px;
}
</style>
