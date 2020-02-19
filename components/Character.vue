<template>
  <div>
    <div class="character-card">
      <div class="character-image">
        <img :src="item.image" alt="Placeholder image" />
      </div>
      <div class="character-content">
        <h2 class="title">{{ item.name }}</h2>
        <div class="basic-info columns">
          <div class="location is-flex column is-7">
            <span class="subtitle">Location</span>
            <span>{{ item.location.name }}</span>
          </div>
          <div class="status is-flex column is-5">
            <span class="subtitle">Status</span>
            <span>{{ item.status }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    item: {
      type: Object
    }
  }
}
</script>

<style lang="scss" scoped>
.character-card {
  border-radius: 10px;
  opacity: 0.6;
  background: white;
  height: 250px;
  display: flex;
  position: relative;
  cursor: pointer;

  &:hover {
    opacity: 1;
    transform: scale(1.05);
  }
}

.character-image {
  background: darkseagreen;
  width: 50%;
  position: relative;
  border-radius: 10px 0 0 10px;
}

img {
  border-radius: 100%;
  position: absolute;
  top: 20%;
  left: 0;
}

.character-content {
  width: 100%;
  padding: 15px 20px;
  font-family: 'Acme', sans-serif;
  position: relative;
}

.basic-info {
  position: absolute;
  bottom: 40px;
  right: 30px;
  left: 30px;
}

.location {
  flex-direction: column;
  span {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
}

.status {
  flex-direction: column;
}
</style>
