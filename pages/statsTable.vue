<template>
  <div
    class="stats-table">
    <table>
      <tr>
        <th/>
        <th
          class="name-title">
          Name
        </th>
        <th
          @click="sort('starts')">
          Starts
        </th>
        <th
          @click="sort('subs')">
          Subs
        </th>
        <th
          @click="sort('wins')">
          Wins
        </th>
        <th
          @click="sort('draws')">
          Draws
        </th>
        <th
          @click="sort('losses')">
          Losses
        </th>
        <th
          @click="sort('goals')">
          Goals
        </th>
        <th
          @click="sort('assists')">
          Assists
        </th>
      </tr>
      <tr
        :key="i"
        v-for="(player, i) in players">
        <td>
          <img
            :src="`http://localhost:8000/asset/${player._id}?asset=player_big.png`"
            class="image">
        </td>
        <td
          class="name">
          {{ player.number }}. {{ player.name }}
        </td>
        <td>
          {{ player.stats.starts }}
        </td>
        <td>
          {{ player.stats.subs }}
        </td>
        <td>
          {{ player.stats.wins }}
        </td>
        <td>
          {{ player.stats.draws }}
        </td>
        <td>
          {{ player.stats.losses }}
        </td>
        <td>
          {{ player.stats.goals }}
        </td>
        <td>
          {{ player.stats.assists }}
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
const axios = require('axios')
export default {
  data () {
    return {
      players: null,
      sortStates: {} // value 0: not sorted, 1: sorted ascending, -1: sorted descending
    }
  },
  created () {
    axios({
      method: 'get',
      url: 'http://localhost:8000/stats/table'
    }).then((res) => {
      const players = res.data

      this.players = Object.values(players)

      const firstPlayerObject = Object.values(players)[0]
      Object.keys(firstPlayerObject.stats).forEach((key) => {
        this.sortStates[key] = 0
      })
    })
  },
  methods: {
    sort (key) {
      const sortFactor = this.sortStates[key] > 0 ? -1 : 1
      this.sortStates[key] = sortFactor

      this.players.sort((playerA, playerB) => {
        return (playerB.stats[key] - playerA.stats[key]) * sortFactor
      })
    }
  }
}
</script>

<style scoped>
table {
  border-collapse: collapse;
}

td {
  text-align: center;
  padding: 0.5rem 1rem;
}

th {
  padding: 1rem;
  cursor: pointer;
}

td:nth-child(odd) {
  background-color: lightgray;
}

td:first-child {
  background-color: initial;
}

.name-title {
  text-align: left;
}

.image {
  width: 6rem;
}

.name {
  text-align: left;
}
</style>
