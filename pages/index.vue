<template>
  <div>
    <div class="navigation">
      <navbar @search="getSearchResults" @filterChanged="getFilteredResults" />
    </div>
    <div class="page-container">
      <div class="inner">
        <div class="container">
          <section class="main">
            <div class="search">
              <search-form
                @search="getSearchResults"
                @filterChanged="getFilteredResults"
                @clearQuery="getSearchResults"
              />
            </div>
            <div class="page-content">
              <div class="columns is-multiline">
                <character-component
                  v-for="character in results"
                  :key="character.id"
                  :item="character"
                  class="character column is-4 is-half-desktop-only"
                />
              </div>
            </div>
            <div class="pagination">
              <base-pagination
                :current-page="currentPage"
                :page-count="pageCount"
                :visible-pages-count="visiblePagesCount"
                class="characters-list__pagination"
                @nextPage="pageChangeHandle('next')"
                @previousPage="pageChangeHandle('previous')"
                @loadPage="pageChangeHandle"
              />
            </div>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from '~/plugins/axios'
import Navbar from '@/components/Navbar.vue'
import CharacterComponent from '@/components/Character.vue'
import BasePagination from '@/components/BasePagination.vue'
import SearchForm from '@/components/SearchForm.vue'

export default {
  watchQuery: true,
  components: {
    Navbar,
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
        context.error({
          statusCode: 404,
          message: 'Uh! No results for the query. Try different query/filters'
        })
      })
  },
  data() {
    return {
      currentPage: Number(this.$route.query.page) || 1
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

.page-content {
  margin-top: 100px;
}

@media screen and (min-width: 769px) {
  .page-content {
    margin-bottom: 30px;
  }

  .pagination {
    justify-content: center;
  }
}

@media screen and (max-width: 768px) {
  .main {
    padding: 150px 30px;
  }

  .page-content {
    margin-top: 10px;
    margin-bottom: 30px;
  }
}

@media screen and (max-width: 400px) {
  .main {
    padding: 150px 20px;
  }

  .page-content {
    margin-top: 50px;
  }

  .character {
    margin-bottom: 100px;
  }
}
</style>
