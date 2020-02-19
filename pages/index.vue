<template>
  <div class="page-container">
    <div class="inner">
      <div class="container">
        <section class="main">
          <search-form
            :species-filter="speciesFilter"
            :search-query="searchQuery"
            @search="getSearchResults"
            @filterChanged="getFilteredResults"
            @clearQuery="getSearchResults"
          />
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
            :visible-pages-count="visiblePagesCount"
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
import SearchForm from '@/components/SearchForm.vue'

export default {
  watchQuery: true,
  components: {
    CharacterComponent,
    BasePagination,
    SearchForm
  },
  asyncData(context) {
    const queryString = Object.keys(context.route.query).reduce((acc, elem) => {
      if (elem !== 'page') acc += '&' + elem + '=' + context.route.query[elem]
      return acc
    }, '')
    const queryPage = context.route.query.page || 1
    return axios
      .get('/character/?page=' + queryPage + queryString)
      .then(({ data }) => {
        return {
          info: data.info,
          results: data.results
        }
      })
      .catch((e) => {
        context.error({ statusCode: 404, message: 'Uh! oh page not found' })
      })
  },
  data() {
    return {
      currentPage: Number(this.$route.query.page) || 1,
      speciesFilter: this.$route.query.species || 'all',
      searchQuery: this.$route.query.name || ''
    }
  },
  computed: {
    pageCount() {
      return this.info.pages
    },
    visiblePagesCount() {
      return this.pageCount > 7 ? 7 : this.pageCount
    }
  },
  methods: {
    getSearchResults(name) {
      const queryObject = { ...this.$route.query }
      if (name === '') {
        delete queryObject.name
      } else {
        queryObject.name = name
      }
      queryObject.page = 1
      this.$router.push({ name: 'index', query: queryObject })
    },
    getFilteredResults(filterSettings) {
      const queryObject = { ...this.$route.query }
      if (filterSettings === 'all') {
        delete queryObject.species
      } else {
        queryObject.species = filterSettings
      }
      queryObject.page = 1
      this.$router.push({ name: 'index', query: queryObject })
    },
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
      const queryObject = { ...this.$route.query }
      queryObject.page = this.currentPage
      this.$router.push({ name: 'index', query: queryObject })
    }
  }
}
</script>

<style lang="scss" scoped>
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

.columns.is-multiline {
  margin-top: 100px;
}
</style>
