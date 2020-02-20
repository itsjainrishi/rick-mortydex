<template>
  <nav class="navbar" role="navigation" aria-label="main-navigation">
    <div class="navbar-brand">
      <nuxt-link class="navbar-item img" to="/">
        <img src="~/assets/R.svg" width="70" @click="closeNavbar" />
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
    </div>
  </nav>
</template>

<script>
import SearchMixin from '@/mixins/SearchMixin.vue'
import BaseSelect from '@/components/BaseSelect.vue'

export default {
  mixins: [SearchMixin],
  components: { BaseSelect },
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
    }
  }
}
</script>

<style scoped lang="scss">
a {
  color: rgb(168, 178, 209);

  &:hover {
    background: transparent;
    color: rgb(100, 255, 218);
  }

  &.nuxt-link-active,
  &.nuxt-link-exact-active {
    background: transparent;
    color: rgb(100, 255, 218);
  }

  &.resume:hover,
  &.resume:focus,
  &.resume:focus-within,
  &.resume:visited {
    background: transparent;
    color: rgb(100, 255, 218);
    border: none;
  }
}

.navbar {
  margin: auto;
  position: fixed;
  width: 100%;
  height: 70px;
  box-shadow: rgba(2, 12, 27, 0.7) 0px 10px 30px -10px;
  background: #fceeb5;
  z-index: 9999;
}

svg {
  height: 18px;
  width: 18px;
  height: 3.0625em;
  line-height: 3.0625em;
}

.navbar-item {
  animation-name: fadeInTop;
  animation-duration: 1s;
  animation-fill-mode: both;
  img {
    max-height: 40px;
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

@media screen and (max-width: 1023px) {
  .navbar-menu {
    box-shadow: rgba(2, 12, 27, 0.7) 0px 10px 30px -10px;
  }
}

@media screen and (max-width: 768px) {
  .navbar-menu {
    background: #fceeb5;
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
}
</style>
