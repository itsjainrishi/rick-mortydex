<template>
  <div :class="['modal', { 'is-active': schwiftyModalActive }]">
    <div class="modal-background"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <button class="delete" aria-label="close" @click="closeModal"></button>
      </header>
      <section class="modal-card-body">
        <div class="Animation">
          <div class="Animation-frames">
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_00.png" />
            </div>
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_01.png" />
            </div>
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_02.png" />
            </div>
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_03.png" />
            </div>
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_04.png" />
            </div>
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_05.png" />
            </div>
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_06.png" />
            </div>
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_07.png" />
            </div>
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_08.png" />
            </div>
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_10.png" />
            </div>
            <div class="Animation-frame">
              <img src="https://muffinman.io/public/img/rm/frame_11.png" />
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isSchwiftyActive: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      schwiftyModalActive: false
    }
  },
  watch: {
    isSchwiftyActive() {
      this.schwiftyModalActive = this.isSchwiftyActive
    }
  },
  methods: {
    closeModal() {
      this.$emit('closeSchwiftyModal')
    }
  }
}
</script>

<style lang="scss" scoped>
$time: 600ms;
$number-of-frames: 12;
$frame-duration: $time / $number-of-frames;

.modal-card-body {
  padding: 0;
  min-height: 645px;
}

@keyframes frame-animation {
  100% {
    transform: translateX(-100%);
  }
}

.Animation {
  margin: 0 auto;
  overflow: hidden;
}

.Animation-frames {
  animation: frame-animation $time infinite;
  animation-timing-function: steps($number-of-frames);
  display: flex;
  width: $number-of-frames * 100%;
  background: #499271;
}

.Animation-frame {
  display: block;
  width: 100% / $number-of-frames;
  max-width: 100% / $number-of-frames;
  flex-basis: 100% / $number-of-frames;

  img {
    width: 100%;
  }
}

@media screen and (max-width: 768px) {
  .modal-card {
    max-height: 1000px;
    padding: 20px;
  }

  .modal-card-body {
    max-height: 1000px;
  }
}

@media screen and (max-width: 550px) {
  .modal-card {
    max-height: 500px;
    padding: 20px;
  }

  .modal-card-body {
    max-height: 500px;
  }
}
</style>
