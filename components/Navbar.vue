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

    <div :class="['navbar-menu', { 'is-active': navbarActive }]"></div>
  </nav>
</template>

<script>
export default {
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
  background: #131517;
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

@media screen and (max-width: 1023px) {
  .navbar-menu {
    background-color: rgb(10, 25, 47);
    box-shadow: rgba(2, 12, 27, 0.7) 0px 10px 30px -10px;
  }
}
</style>
