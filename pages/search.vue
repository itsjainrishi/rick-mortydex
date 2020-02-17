<template>
  <div class="page-container">
    <div class="inner">
      <div class="container">
        <section class="main">
          <div class="columns is-multiline">
            <character-component
              v-for="character in characters.results"
              :key="character.id"
              :item="character"
              class="column is-4"
            />
            <button @click="search">Search</button>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
import axios from '~/plugins/axios'
import CharacterComponent from '@/components/Character.vue'

export default {
  components: {
    CharacterComponent
  },
  data() {
    return {
      characters: {}
    }
  },
  methods: {
    async search() {
      const { data } = await axios.get(
        'https://rickandmortyapi.com/api/character/?page=19'
      )
      console.log(data)
      // this.characters = { characters }
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
