<template>
  <nav class="navbar" role="navigation" aria-label="main-navigation">
    <div class="navbar-brand">
      <nuxt-link class="navbar-item img" to="/">
        <img
          src="~/assets/rickandmorty.png"
          width="150px"
          @click="closeNavbar"
        />
      </nuxt-link>

      <a
        :class="['navbar-burger', 'burger', { 'is-active': navbarActive }]"
        role="button"
        @click="toggleNavbar"
      >
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
      </a>
    </div>

    <div :class="['navbar-menu', { 'is-active': navbarActive }]">
      <div class="navbar-start">
        <div class="navbar-item is-hidden-desktop touch-search">
          <form @submit.prevent="onSubmit">
            <div class="field">
              <p class="control">
                <input
                  v-model="name"
                  class="input"
                  type="text"
                  placeholder="Search by Name"
                />
              </p>
            </div>
          </form>
        </div>
        <div class="navbar-item is-hidden-desktop touch-search">
          <div>
            <div v-if="searchQueryReference.length" class="query field">
              <label class="label">Search Query</label>
              <div class="control">
                <div class="name box">
                  <span class="query-span">{{ searchQueryReference }}</span>
                  <span class="icon" @click="clearQuery">&times;</span>
                </div>
              </div>
            </div>
            <div class="field">
              <label class="label">Species Filter:</label>
              <div class="control">
                <base-select
                  :options="filters"
                  @filterChanged="onFilterChange"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="navbar-end">
        <div class="navbar-item schwifty">
          <button
            class="btn-schwifty"
            :disabled="schwiftyActive"
            @click="openSchwifty"
          >
            Get Schwifty
          </button>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
import SearchMixin from '@/mixins/SearchMixin.vue'
import BaseSelect from '@/components/BaseSelect.vue'

export default {
  components: { BaseSelect },
  mixins: [SearchMixin],
  props: {
    schwiftyActive: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      navbarActive: false
    }
  },
  methods: {
    toggleNavbar() {
      this.navbarActive = !this.navbarActive
    },
    closeNavbar() {
      this.navbarActive = false
    },
    onFilterChange(value) {
      this.$emit('filterChanged', value)
    },
    openSchwifty() {
      this.closeNavbar()
      this.$emit('openSchwiftyModal')
    }
  }
}
</script>

<style scoped lang="scss">
.navbar {
  margin: auto;
  width: 100%;
  height: 70px;
  box-shadow: rgba(2, 12, 27, 0.7) 0px 10px 30px -10px;
  background: #fceeb5;
  z-index: 9999;
  font-family: 'Acme', sans-serif;
}

.navbar-burger {
  height: 70px;
}

.navbar-item {
  animation-name: fadeInTop;
  animation-duration: 1s;
  animation-fill-mode: both;
  padding: 0;

  .navbar-link {
    padding: 0;
  }

  img {
    max-height: 70px;
  }
}

.query.field {
  line-height: 40px;

  .control {
    width: 100px;
  }

  .box {
    padding: 0 10px;
  }
}

.name.box {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;

  span:not(.icon) {
    transition: all 300ms ease;
    font-size: 1em;
    user-select: none;
    color: green;
    max-width: 70px;
  }

  span.icon {
    position: absolute;
    top: 8px;
    right: 0;
    font-size: 1.5rem;
    color: red;
  }
}

.btn-schwifty {
  color: darkseagreen;
  padding: 10px;
  font-size: 20px;
  background: transparent;
  border: 2px solid darkseagreen;
  border-radius: 3px;
  font-family: 'Acme', sans-serif;
  cursor: pointer;

  &:disabled {
    cursor: not-allowed;
  }
}

@media screen and (max-width: 1023px) {
  .navbar-menu {
    box-shadow: rgba(2, 12, 27, 0.7) 0px 10px 30px -10px;
    background: #fceeb5;
    padding: 10px;
  }

  .touch-search .input {
    border: none;
    &:focus,
    &:focus-within {
      outline: 0;
      border: none;
      box-shadow: none;
    }
  }

  .btn-schwifty {
    margin-top: 20px;
  }
}
</style>
