<template>
  <div
    class="edit-league">
    <table>
      <tr>
        <th>
          Pos
        </th>
        <th>
          Team
        </th>
        <th>
          Played
        </th>
        <th>
          Wins
        </th>
        <th>
          Draws
        </th>
        <th>
          Losses
        </th>
        <th>
          Goals For
        </th>
        <th>
          Goals Against
        </th>
      </tr>
      <tr
        v-for="(team, i) in data"
        :key="i">
        <td>
          <input
            type="number"
            :value="team.position"
            @input="onInput($event, 'position', i)">
        </td>
        <td>
          <div
            class="team">
            <div
              class="logo-wrapper">
              <img
                class="logo"
                :src="require(`@/assets/logos/teams/${team.asset}`)"/>
            </div>
            <span
              class="name">
              {{ team.name }}
            </span>
          </div>
        </td>
        <td>
          <input
            type="number"
            :value="team.played"
            @input="onInput($event, 'played', i)">
        </td>
        <td>
          <input
            type="number"
            :value="team.wins"
            @input="onInput($event, 'wins', i)">
        </td>
        <td>
          <input
            type="number"
            :value="team.draws"
            @input="onInput($event, 'draws', i)">
        </td>
        <td>
          <input
            type="number"
            :value="team.losses"
            @input="onInput($event, 'losses', i)">
        </td>
        <td>
          <input
            type="number"
            :value="team.goalsFor"
            @input="onInput($event, 'goalsFor', i)">
        </td>
        <td>
          <input
            type="number"
            :value="team.goalsAgainst"
            @input="onInput($event, 'goalsAgainst', i)">
        </td>
      </tr>
    </table>
    <div
      style="text-align: center">
      <div
        class="save"
        @click="saveClicked">Save</div>
    </div>
  </div>
</template>

<script>
const axios = require('axios')

export default {
  data () {
    return {
      data: null,
      newData: null
    }
  },
  created () {
    this.getLeagueTable()
  },
  methods: {
    getLeagueTable () {
      axios({
        method: 'get',
        url: 'http://localhost:8000/leagueTable'
      }).then((res) => {
        if (!res.data) { return }

        const teams = res.data.teams

        this.data = teams

        this.newData = teams
      })
    },
    saveClicked () {
      const teams = this.newData

      axios({
        method: 'post',
        url: 'http://localhost:8000/leagueTable/update',
        data: {
          teams
        }
      }).then((res) => {
        if (!res.data) { return }

        const success = res.data.success

        if (!success) { return }

        this.getLeagueTable()
      })
    },
    onInput (e, key, i) {
      const value = e.target.value

      this.newData[i][key] = parseInt(value)
    }
  }
}
</script>

<style scoped>
  .team {
    display: flex;
    align-items: center;
  }

  .logo-wrapper {
    width: 3rem;
    text-align: center;
  }

  .logo {
    height: 2.5rem;
  }

  .name {
    margin-left: 1rem;
  }

  input {
    width: 3rem;
  }

  td,th {
    padding: 0.5rem 1.5rem;
    text-align: left;
  }

  .save {
    display: inline-block;
    padding: 0.5rem 4rem;
    color: white;
    background-color: #00a000;
    border-radius: 5px;
    cursor: pointer;
  }
</style>
