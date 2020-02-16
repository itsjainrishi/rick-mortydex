<template>
  <div class="page-container">
    <div class="inner">
      <div></div>
    </div>
  </div>
</template>

<script>
import charactersQuery from '~/apollo/queries/character/characters'

export default {
  data() {
    return {
      characters: [],
      query: ''
    }
  },
  apollo: {
    characters: {
      prefetch: true,
      query: charactersQuery
    }
  },
  mounted() {
    console.log(this.characters)
  }
}
</script>

<style>
.page-container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  background: url('~assets/rick-morty.png');
  background-size: cover;
}

.inner {
  min-height: 100vh;
  width: 100%;
  background-color: hsla(217, 26%, 27%, 0.85);
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
