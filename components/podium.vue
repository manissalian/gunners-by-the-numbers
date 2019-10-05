<template>
  <div
    class="podium">
    <div
      v-for="(player, i) in data"
      :key="i"
      class="item"
      @click="playerClicked">
      <div
        class="img-wrapper">
        <img
          :src="`http://localhost:8000/asset/${player._id}?asset=player.png`">
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
        <div style="text-align: center">{{ player.number }}. {{ player.name }}</div>
        <div
          style="
            text-align: center;
            text-transform: capitalize
          ">{{ player.metric }} {{ type }}{{ player[type] > 1 ? 's' : '' }}</div>
      </div>
    </div>
  </div>
</template>

<script>
const axios = require('axios')

export default {
  props: ['type'],
  data () {
    return {
      data: null
    }
  },
  created () {
    const type = this.type

    axios({
      method: 'get',
      url: 'http://localhost:8000/podium/' + type
    }).then((res) => {
      const data = res.data
      const playerIds = Object.keys(data)

      Promise.all(playerIds.map((id) => {
        return axios({
          method: 'get',
          url: 'http://localhost:8000/player/id?id=' + id
        })
      })).then((res) => {
        const players = res.map((player) => {
          const playerData = player.data
          const metric = data[playerData._id]

          return {
            ...playerData,
            metric
          }
        }).sort((playerA, playerB) => {
          return playerB.metric - playerA.metric
        }).slice(0, 3)

        this.data = players
      })
    })
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
    cursor: pointer;
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
    width: 85%;
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
