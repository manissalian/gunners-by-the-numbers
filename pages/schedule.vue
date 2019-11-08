<template>
  <div
    class="schedule">
    <next-match/>
    <div
      class="title">Upcoming Matches</div>
    <div
      v-for="(match, i) in data"
      :key="i"
      class="match">
      <div
        class="tournament-and-date">
        <img
          class="tournament-logo"
          :style="{
            height: match.doc.tournament === 'europaLeague' ? '4.5rem' : 'auto',
            width: match.doc.tournament === 'premierLeague' ? '6rem'
            : match.doc.tournament === 'leagueCup' ? '3rem' : 'auto'
          }"
          :src="require(`@/assets/logos/tournaments/${match.doc.tournament}.png`)"/>
        <div
          class="date">{{ dateFormat(match.doc.date, 'dddd, mmmm dS, yyyy, h:MM:ss TT') }}</div>
        <div
          class="stadium">{{ match.doc.ground.name }}</div>
      </div>
      <div
        class="opponent">
        <div
          class="opponent-logo-wrapper">
          <img
            class="opponent-logo"
            :src="`http://localhost:8000/asset/${match.doc._id}?asset=opponentLogo`"/>
        </div>
        <div
          class="opponent-title">{{ match.doc.opponent.title }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import NextMatch from '@/components/nextMatch'

const axios = require('axios')
const dateFormat = require('dateformat')

export default {
  components: {
    NextMatch
  },
  data () {
    return {
      data: null
    }
  },
  beforeCreate () {
    axios({
      method: 'get',
      url: 'http://localhost:8000/match/all'
    }).then((res) => {
      const rows = res.data.rows

      const pendingRows = rows.filter((row) => {
        return row.doc.result === 'pending'
      })

      pendingRows.sort((rowA, rowB) => {
        return new Date(rowA.doc.date) - new Date(rowB.doc.date)
      })

      pendingRows.shift()

      this.data = pendingRows
    })
  },
  methods: {
    dateFormat
  }
}
</script>

<style scoped>
.title {
  margin-top: 1.5rem;
  text-align: center;
  font-size: 1.5rem;
}

.match {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 1rem 0rem;
  max-width: 500px;
  height: 200px;
  margin-top: 1.5rem;
  margin-left: auto;
  margin-right: auto;
  align-items: center;
  background-color: silver;
  text-align: center;
  border-radius: 0.5rem;
}

.match > *:first-child {
  margin-top: 0rem;
}

.opponent {
  width: 10rem;
  margin-top: 1rem;
}

.opponent-title {
  margin-top: 0.5rem;
}

.opponent-logo-wrapper {
  height: 5rem;
}

.opponent-logo {
  height: 100%
}

.date {
  max-width: 200px;
  margin-top: 0.5rem;
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

@media screen and (min-width: 768px) {
  .match {
    flex-direction: row;
  }

  .opponent {
    margin-left: 4rem;
    margin-top: initial;
  }
}
</style>
