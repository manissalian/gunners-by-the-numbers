<template>
  <div
    class="player-profile">
    <div
      class="top">
      <img
        :src="`http://localhost:8000/asset/${playerId}?asset=player_big.png`">
      <div
        class="info">
        <div
          class="title">{{ player.number }}. {{ player.name }}</div>
        <table>
          <tbody>
            <tr>
              <th>Games:</th>
              <td>{{ starts }} (+{{ subs }})</td>
            </tr>
            <tr>
              <th>Wins:</th>
              <td>{{ wins }}</td>
            </tr>
            <tr>
              <th>Draws:</th>
              <td>{{ draws }}</td>
            </tr>
            <tr>
              <th>Losses:</th>
              <td>{{ losses }}</td>
            </tr>
            <tr>
              <th>Goals:</th>
              <td>{{ goals }}</td>
            </tr>
            <tr>
              <th>Assists:</th>
              <td>{{ assists }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <MatchList
      :playerId="playerId"
      :matches="matches"/>
    </div>
  </div>
</template>

<script>
import MatchList from '@/components/matchList'
const axios = require('axios')

export default {
  components: {
    MatchList
  },
  data () {
    const playerId = this.$route.params.id

    return {
      playerId,
      player: {
        name: null,
        number: null
      },
      matches: null,
      starts: 0,
      subs: 0,
      wins: 0,
      draws: 0,
      losses: 0,
      goals: 0,
      assists: 0
    }
  },
  created () {
    const id = this.playerId
    axios({
      method: 'get',
      url: 'http://localhost:8000/player/' + id
    }).then((res) => {
      const player = res.data
      this.player = player

      axios({
        method: 'get',
        url: 'http://localhost:8000/match/player/' + id
      }).then((res) => {
        const matches = res.data
        this.matches = matches

        this.starts = matches.reduce((total, match) => {
          return total + (match.doc.players.includes(id) ? 1 : 0)
        }, 0)

        this.subs = matches.reduce((total, match) => {
          return total + (match.doc.subs.includes(id) ? 1 : 0)
        }, 0)

        this.wins = matches.reduce((total, match) => {
          return total + (match.doc.result === 'win' ? 1 : 0)
        }, 0)

        this.draws = matches.reduce((total, match) => {
          return total + (match.doc.result === 'draw' ? 1 : 0)
        }, 0)

        this.losses = matches.reduce((total, match) => {
          return total + (match.doc.result === 'lose' ? 1 : 0)
        }, 0)

        this.goals = matches.reduce((total, match) => {
          const goals = match.doc.goals
          const scored = goals.filter((goal) => {
            return goal.goal === id
          })
          return total + scored.length
        }, 0)

        this.assists = matches.reduce((total, match) => {
          const goals = match.doc.goals
          const assisted = goals.filter((goal) => {
            return goal.assist === id
          })
          return total + assisted.length
        }, 0)
      })
    })
  }
}
</script>

<style scoped>
  .top {
    display: flex;
    flex-direction: column;
    background-color: brown;
    border-radius: 0.5rem;
  }

  .top img {
    width: 100%;
    max-width: 500px;
    padding: 1rem;
    margin-left: auto;
    margin-right: auto;
  }

  .top .info {
    flex: 1;
    padding: 1rem 2rem;
    background-color: black;
    border-radius: 0rem 0rem 0.5rem 0.5rem;
    color: #dddddd;
  }

  .top .title {
    font-size: 1.75rem;
    text-align: center;
    border-bottom: 0.15rem solid #dddddd;
  }

  .top table {
    text-align: left;
    font-size: 1.25rem;
    padding-top: 2rem;
    margin-left: auto;
    margin-right: auto;
  }

  .top td {
    padding-left: 4rem;
  }

  @media screen and (min-width: 1024px) {
    .top {
      flex-direction: row;
    }

    .top img {
      width: auto;
    }

    .top .info {
      border-radius: 0rem 0.5rem 0.5rem 0rem;
    }

    .top .title {
      font-size: 2.25rem;
    }
  }
</style>
