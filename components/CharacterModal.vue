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
          <div class="location">
            <span>Location:</span>
            <span>{{ characterInfo.location.name }}</span>
          </div>
          <div class="status">
            <span>Status:</span>
            <span>{{ characterInfo.status }}</span>
          </div>
          <div class="origin">
            <span>Origin:</span>
            <span>{{ characterInfo.origin.name }}</span>
          </div>
          <div class="species">
            <span>Species:</span>
            <span>{{ characterInfo.species }}</span>
          </div>
          <div class="gender">
            <span>Gender:</span>
            <span>{{ characterInfo.gender }}</span>
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

.modal-card-body {
  min-height: 450px;
  background: #fceeb5;
}

.character-info {
  position: relative;
  span {
    &:nth-child(2n + 1) {
      position: absolute;
      left: 160px;
    }

    &:nth-child(2n) {
      position: absolute;
      left: 250px;
    }
  }
}

.location {
  span {
    top: 10px;
  }
}

.status {
  span {
    top: 30px;
  }
}

.origin {
  span {
    top: 50px;
  }
}

.species {
  span {
    top: 70px;
  }
}

.gender {
  span {
    top: 90px;
  }
}
</style>
