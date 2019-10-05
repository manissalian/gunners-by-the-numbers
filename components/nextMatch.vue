<template>
  <div
    v-if="data"
    class="next-match">
    <img
      class="tournament-logo"
      :style="{
        height: data.tournament === 'europaLeague' ? '4.5rem' : 'auto',
        width: data.tournament === 'premierLeague' ? '5.5rem'
        : data.tournament === 'leagueCup' ? '3rem' : 'auto'
      }"
      :src="require(`@/assets/logos/tournaments/${data.tournament}.png`)"/>
    <div
      class="title">Next Match</div>
    <div
      class="date">{{ formatDate(new Date(data.date)) }}</div>
    <div
      class="stadium">{{ data.ground.name }}</div>
    <div
      :style="{
        direction: data.ground.type === 'away' ? 'rtl' : 'ltr'
      }"
      class="row">
      <div
        class="team-wrapper">
        <div
          class="logo-wrapper">
          <img
            class="logo"
            src="@/assets/logos/teams/arsenal.svg"/>
        </div>
        <div
          class="team">Arsenal</div>
      </div>
      <div
        class="vs">VS</div>
      <div
        class="team-wrapper">
        <div
          class="logo-wrapper">
          <img
            class="logo"
            :src="`http://localhost:8000/asset/${data._id}?asset=opponentLogo`"/>
        </div>
        <div
          class="team">{{ data.opponent.title }}</div>
      </div>
    </div>
  </div>
</template>

<script>
const axios = require('axios')
const dateFormat = require('dateformat')

export default {
  data () {
    return {
      data: null
    }
  },
  beforeCreate () {
    axios({
      method: 'get',
      url: 'http://localhost:8000/match/next'
    }).then((res) => {
      const doc = res.data.doc

      this.data = doc
    })
  },
  methods: {
    formatDate (date) {
      return dateFormat(date, 'dddd, mmmm dS, yyyy, h:MM:ss TT')
    }
  }
}
</script>

<style scoped>
  .next-match {
    position: relative;
    max-width: 768px;
    margin-left: auto;
    margin-right: auto;
  }

  .tournament-logo {
    position: absolute;
  }

  .title {
    text-align: center;
    font-size: 1.5rem;
  }

  .date {
    max-width: 200px;
    margin-left: auto;
    margin-right: auto;
    font-size: 0.9rem;
    text-align: center;
  }

  .stadium {
    text-align: center;
    font-size: 0.85rem;
    color: #696969;
  }

  .row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    margin-top: 0.5rem;
  }

  .team-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .team {
    margin-top: 0.5rem;
  }

  .logo-wrapper {
    width: 7rem;
    height: 7rem;
    text-align: center;
  }

  .logo {
    height: 100%;
  }

  .vs {
    font-size: 1.25rem;
  }
</style>
