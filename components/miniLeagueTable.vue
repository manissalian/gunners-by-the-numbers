<template>
  <div
    class="mini-league-table">
    <img
      class="title"
      src="@/assets/logos/tournaments/premierLeague.png"/>
    <table>
      <tr
        v-for="(team, i) in data"
        :key="i"
        class="team">
        <td
          class="position">{{ team.position }}</td>
        <td
          style="text-align: center">
          <img
            :src="`http://localhost:8000/asset/leagueTable?asset=${team.asset}`"
            class="logo"/>
        </td>
        <td
          class="name">{{ team.name }}</td>
        <td
          class="games">{{ team.played }}</td>
        <td
          class="diff">{{ team.goalsFor - team.goalsAgainst > 0 ? '+' : '' }}
          {{ team.goalsFor - team.goalsAgainst }}</td>
        <td
          class="points">{{ (team.wins * 3) + team.draws }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
const axios = require('axios')

export default {
  data () {
    return {
      data: null
    }
  },
  beforeCreate () {
    axios({
      method: 'get',
      url: 'http://localhost:8000/leagueTable'
    }).then((res) => {
      if (!res.data) { return }

      const teams = res.data.teams

      this.data = teams
    })
  }
}
</script>

<style scoped>
  .mini-league-table {
    display: none;
    font-size: 0.8rem;
  }

  td {
    text-align: center;
  }

  .title {
    width: 7rem;
  }

  .logo {
    height: 1.2rem;
    margin-top: 0.4rem;
    padding-left: 0.5rem;
  }

  .name {
    padding-left: 0.5rem;
    padding-right: 1rem;
    margin-left: 0.5rem;
    text-align: left;
  }

  .diff,
  .points {
    padding-left: 0.5rem;
  }

  @media screen and (min-width: 1280px) {
    .mini-league-table {
      display: block;
    }
  }
</style>
