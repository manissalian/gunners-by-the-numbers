<template>
  <div
    class="podium">
    <div
      v-for="(p, i) in data"
      :key="i"
      class="item"
      @click="playerClicked">
      <div
        class="img-wrapper">
        <img
          src="@/assets/players/aubameyang.png"/>
      </div>
      <div
        :style="{
          color: i === 0 ? 'gold' : i === 1 ? 'silver' : '#cd7f32',
          paddingBottom: i === 2 ? '0.5rem' : 'initial'
        }"
        class="rank">{{ i + 1}}</div>
      <div
        :style="{
          color: i === 0 ? 'gold' : i === 1 ? 'silver' : '#cd7f32'
        }"
        class="info">
        <div style="text-align: center">Pierre Emerick Aubameyang</div>
        <div style="text-align: center">32 Goals</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      data: [0, 1, 2]
    }
  },
  methods: {
    playerClicked () {
      this.$router.push('/playerProfile')
    }
  }
}
</script>

<style scoped>
  .podium {
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .item {
    position: relative;
    flex: 1;
    margin-top: 2rem;
  }

  .img-wrapper {
    text-align: center;
  }

  img {
    width: 220px;
  }

  .rank {
    position: absolute;
    left: 0;
    top: 0;
    width: 4.5rem;
    height: 6rem;
    line-height: 6rem;
    text-align: center;
    font-size: 6rem;
    background-color: rgba(0, 0, 0, 0.75);
    border-radius: 0.5rem;
  }

  .info {
    position: absolute;
    width: 75%;
    left: 50%;
    transform: translateX(-50%);
    bottom: 1.5rem;
    padding: 0.5rem 0rem;
    border-radius: 0.5rem;
    background-color: rgba(0, 0, 0, 0.75);
  }

  @media screen and (min-width: 768px) {
    .podium {
      flex-direction: row;
    }

    .rank {
      font-size: 4rem;
      transform: scale(0.7);
    }
  }
</style>
