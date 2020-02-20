<template>
  <div :class="['modal', { 'is-active': modalActive }]">
    <div class="modal-background"></div>
    <div v-if="characterInfo.name" class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title">{{ characterInfo.name }}</p>
        <button class="delete" @click="closeModal" aria-label="close"></button>
      </header>
      <section class="modal-card-body">
        <div class="character-image">
          <figure>
            <img :src="characterInfo.image" />
          </figure>
        </div>
        <div class="character-info">
          <div class="location columns is-mobile">
            <span
              class="column is-offset-3-desktop is-2-mobile is-2-desktop is-offset-3-mobile"
            >
              Location:
            </span>
            <span class="column">{{ characterInfo.location.name }}</span>
          </div>
          <div class="status columns is-mobile">
            <span
              class="column is-offset-3-desktop is-2-mobile is-2-desktop is-offset-3-mobile"
            >
              Status:
            </span>
            <span class="column">{{ characterInfo.status }}</span>
          </div>
          <div class="origin columns is-mobile">
            <span
              class="column is-offset-3-desktop is-2-mobile is-2-desktop is-offset-3-mobile"
            >
              Origin:
            </span>
            <span class="column">{{ characterInfo.origin.name }}</span>
          </div>
          <div class="species columns is-mobile">
            <span
              class="column is-offset-3-desktop is-2-mobile is-2-desktop is-offset-3-mobile"
            >
              Species:
            </span>
            <span class="column">{{ characterInfo.species }}</span>
          </div>
          <div class="gender columns is-mobile">
            <span
              class="column is-offset-3-desktop is-2-mobile is-2-desktop is-offset-3-mobile"
            >
              Gender:
            </span>
            <span class="column">{{ characterInfo.gender }}</span>
          </div>
        </div>

        <!-- Content ... -->
      </section>
      <footer class="modal-card-foot">
        <button class="button" @click="closeModal">Cancel</button>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isActive: {
      type: Boolean,
      default: false
    },
    characterInfo: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      modalActive: false
    }
  },
  watch: {
    isActive() {
      this.modalActive = this.isActive
    }
  },
  methods: {
    closeModal() {
      this.$emit('closeModal')
    }
  }
}
</script>

<style lang="scss" scoped>
.modal {
  font-family: 'Acme', sans-serif;
}

.modal-card {
  padding: 20px;
}

.modal-card-body {
  min-height: 450px;
  background: #fceeb5;
}

.character-info {
  .columns {
    margin: 0;
  }

  .column {
    padding: 0 10px;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  span {
    text-align: left;
    white-space: nowrap;
  }
}

@media screen and (max-width: 550px) {
  .column.is-offset-3-mobile {
    margin-left: 2%;
  }

  .column.is-2-mobile {
    width: 40%;
  }
}
</style>
