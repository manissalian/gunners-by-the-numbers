<template>
  <div
    class="match-list">
    <div
      v-for="(match, i) in data"
      :key="i"
      :style="{
        backgroundColor: match.doc.result === 'win' ? 'rgba(0, 160, 0, 0.35)'
          : match.doc.result === 'draw' ? 'rgba(240, 160, 0, 0.35)'
          : match.doc.result === 'lose' ? 'rgba(240, 0, 0, 0.35)'
          : 'none'
      }"
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
          class="date">{{ dateFormat(match.doc.date, 'yyyy-mm-dd') }}</div>
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
      <div
        class="ground">
        <div
          class="ground-type">{{ match.doc.ground.type }}</div>
        <div
          class="ground-name">{{ match.doc.ground.name }}</div>
      </div>
      <div
        class="outcome">
        <div
          class="result">{{ match.doc.result }}</div>
        <div
          class="score">{{ match.doc.goals.length }} - {{ match.doc.opponent.goals }}</div>
      </div>
      <div
        v-if="playerId"
        class="player-stats">
        <div
          class="goals">
          <div
            class="goals-logo-wrapper">
            <img
              class="goals-logo"
              src="@/assets/logos/goal.png"/>
          </div>
          <div
            class="goals-count">{{ getGoals(match) }}</div>
        </div>
        <div
          class="assists">
          <div
            class="assists-logo-wrapper">
            <img
              class="assists-logo"
              src="@/assets/logos/assist.png"/>
          </div>
          <div
            class="assists-count">{{ getAssists(match) }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const dateFormat = require('dateformat')

export default {
  props: [
    'playerId',
    'matches'
  ],
  computed: {
    data () {
      const unsortedMatches = this.matches

      return unsortedMatches && unsortedMatches.sort((matchA, matchB) => {
        return new Date(matchB.doc.date) - new Date(matchA.doc.date)
      })
    }
  },
  methods: {
    dateFormat,
    getGoals (match) {
      const goals = match.doc.goals.filter((goal) => {
        return goal.goal === this.playerId
      }).length

      return goals
    },
    getAssists (match) {
      const assists = match.doc.goals.filter((goal) => {
        return goal.assist === this.playerId
      }).length

      return assists
    }
  }
}
</script>

<style scoped>
.match {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  padding: 1rem 0rem;
  margin-top: 1.5rem;
  text-align: center;
  border-radius: 0.5rem;
}

.match > * {
  margin-top: 1rem;
}

.match > *:first-child {
  margin-top: 0rem;
}

.tournament-and-date {
  width: 8rem;
}

.opponent {
  width: 10rem;
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

.ground {
  width: 16rem;
}

.ground-type,
.outcome {
  text-transform: uppercase;
  font-weight: bold;
  font-size: 1.15rem;
}

.player-stats {
  display: flex;
}

.goals,
.assists {
  margin: 0 0.5rem;
}

.goals-logo-wrapper,
.assists-logo-wrapper {
  height: 3rem;
}

.goals-logo {
  width: 2rem;
}

.assists-logo {
  width: 2.75rem;
}

@media screen and (min-width: 768px) {
  .match {
    flex-direction: row;
  }
}
</style>
