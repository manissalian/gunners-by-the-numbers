<template>
  <div
    v-if="data"
    class="next-match">
    <img
      class="tournament-logo"
      src="@/assets/logos/tournaments/premierLeague.png"/>
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
            :src="require(`@/assets/logos/teams/${data.opponent.asset}`)"/>
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
    width: 13%;
    min-width: 5rem;
  }

  .title {
    text-align: center;
    font-size: 1.5rem;
  }

  .date {
    text-align: center;
  }

  .stadium {
    text-align: center;
    font-size: 0.9rem;
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
    text-align: center;
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
