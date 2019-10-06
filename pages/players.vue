<template>
  <div
    class="players">
    <div
      v-for="(player, i) in players"
      :key="i"
      class="player"
      @click="playerClicked(player.doc._id)">
      <img
        :src="`http://localhost:8000/asset/${player.doc._id}?asset=player.png`">
      <div
        class="name">{{ player.doc.number }}. {{ player.doc.name }}</div>
    </div>
  </div>
</template>

<script>
const axios = require('axios')
export default {
  data () {
    return {
      players: null
    }
  },
  created () {
    axios({
      method: 'get',
      url: 'http://localhost:8000/player/all'
    }).then((res) => {
      const players = res.data.rows
      this.players = players
    })
  },
  methods: {
    playerClicked (id) {
      this.$router.push(`/playerProfile/${id}`)
    }
  }
}
</script>

<style scoped>
  .player {
    display: inline-block;
    width: 100%;
    padding-top: 1rem;
    padding-bottom: 0.5rem;
    margin-bottom: 2rem;
    text-align: center;
    background-color: #f8f8ff;
    border-bottom: 2px solid #f00000;
    cursor: pointer;
    transition: all 0.1s;
  }

  .player:hover {
    transform: scale(1.05);
  }

  .name {
    font-weight: bold;
    font-size: 16px !important;
  }

  .separator {
    height: 2px;
    background-color: #f00000;
  }

  @media screen and (min-width: 768px) {
    .player {
      width: 48%;
      margin-bottom: 4%;
    }

    .player:nth-child(odd) {
      margin-right: 4%;
    }
  }

  @media screen and (min-width: 1024px) {
    .player {
      width: 30.66%;
      margin-right: 4%;
    }

    .player:nth-child(3n) {
      margin-right: 0%;
    }
  }

  @media screen and (min-width: 1280px) {
    .player {
      width: 22%;
      margin-right: 4% !important;
    }

    .player:nth-child(4n) {
      margin-right: 0% !important;
    }
  }
</style>
