<template>
  <div
    class="admin-create-match">
    <div
      class="title">Opponent</div>
    <input
      type="text"
      placeholder="Title"
      @input="opponentTitleInput">
    <input
      type="text"
      placeholder="Asset"
      @input="opponentAssetInput">
    <input
      type="number"
      placeholder="Goals"
      @input="opponentGoalsInput">

    <div
      class="title">Result</div>
    <select
      @change="resultChanged">
      <option
        value="pending">Pending</option>
      <option
        value="win">Win</option>
      <option
        value="lose">Lose</option>
      <option
        value="draw">Draw</option>
    </select>

    <div
      class="title">Ground</div>
    <input
      type="text"
      placeholder="Name"
      @input="groundNameInput">
    <select
      @change="groundTypeChanged">
      <option
        value="home">Home</option>
      <option
        value="away">Away</option>
      <option
        value="neutral">Neutral</option>
    </select>

    <div
      class="title">Date</div>
    <input
      type="datetime-local"
      @change="dateChanged">

    <div
      class="title">Tournament</div>
    <select
      @change="tournamentChanged">
      <option
        value="premierLeague">Premier League</option>
      <option
        value="europaLeague">Europa League</option>
      <option
        value="faCup">Fa Cup</option>
      <option
        value="leagueCup">League Cup</option>
    </select>

    <div
      class="title">Players</div>
    <div
      class="player"
      v-for="(player, i) in playersData"
      :key="`player${i}`">
      <div
        class="player-inner">
        <input
          type="checkbox"
          @change="playerCheckboxChanged($event, player.doc._id)">
        <div
          class="player-name">{{ player.doc.name }}</div>
      </div>
    </div>

    <div
      class="title">Subs</div>
    <div
      class="player"
      v-for="(player, i) in playersData"
      :key="`sub${i}`">
      <div
        class="player-inner">
        <input
          type="checkbox"
          @change="subCheckboxChanged($event, player.doc._id)">
        <div
          class="player-name">{{ player.doc.name }}</div>
      </div>
    </div>

    <div
      class="title">
      Goals
      <span
        class="add"
        @click="addGoalClicked">Add +</span>
    </div>
    <div
      class="goal"
      v-for="(goal, i) in goals"
      :key="goal.timestamp">
      <div>Goal:</div>
      <select
        @change="scorerSelected($event, i)">
        <option
          v-for="(player, i) in playersData"
          :key="`goalScorer${i}`"
          :name="player.doc.name"
          :value="player.doc._id">{{ player.doc.name }}</option>
        <option
          value="ownGoal">Own Goal</option>
      </select>
      <div>Assist:</div>
      <select
        @change="assisterSelected($event, i)">
        <option
          :value="null">None</option>
        <option
          v-for="(player, i) in playersData"
          :key="`goalAssister${i}`"
          :name="player.doc.name"
          :value="player.doc._id">{{ player.doc.name }}</option>
      </select>
      <div
        class="delete"
        @click="deleteClicked(i)">Delete</div>
    </div>

    <div
      style="text-align: center">
      <div
        class="create"
        @click="createClicked">Create</div>
    </div>
  </div>
</template>

<script>
const axios = require('axios')

export default {
  data () {
    return {
      playersData: null,
      opponent: {
        title: null,
        asset: null,
        goals: 0
      },
      result: 'pending',
      ground: {
        name: null,
        type: 'home'
      },
      date: null,
      tournament: 'premierLeague',
      players: [],
      subs: [],
      goals: []
    }
  },
  beforeCreate () {
    axios({
      method: 'get',
      url: 'http://localhost:8000/player/all'
    }).then((res) => {
      const rows = res.data.rows

      this.playersData = rows.filter((row) => {
        return row.doc.status === 'active'
      })
    })
  },
  methods: {
    opponentTitleInput (e) {
      const opponentTitle = e.target.value

      this.opponent.title = opponentTitle
    },
    opponentAssetInput (e) {
      const opponentAsset = e.target.value

      this.opponent.asset = opponentAsset
    },
    opponentGoalsInput (e) {
      const opponentGoals = e.target.value

      this.opponent.goals = opponentGoals
    },
    resultChanged (e) {
      const result = e.target.value

      this.result = result
    },
    groundNameInput (e) {
      const groundName = e.target.value

      this.ground.name = groundName
    },
    groundTypeChanged (e) {
      const groundType = e.target.value

      this.ground.type = groundType
    },
    dateChanged (e) {
      const date = e.target.value

      this.date = date
    },
    tournamentChanged (e) {
      const tournament = e.target.value

      this.tournament = tournament
    },
    playerCheckboxChanged (e, playerId) {
      const checked = e.target.checked

      if (checked) {
        this.players.push(playerId)
      } else {
        const index = this.players.indexOf(playerId)
        this.players.splice(index, 1)
      }
    },
    subCheckboxChanged (e, playerId) {
      const checked = e.target.checked

      if (checked) {
        this.subs.push(playerId)
      } else {
        const index = this.players.indexOf(playerId)
        this.subs.splice(index, 1)
      }
    },
    addGoalClicked () {
      const goal = {
        goal: this.playersData[0].doc.name,
        assist: null,
        timestamp: JSON.stringify(new Date()) // help render correctly after splicing
      }

      this.goals.push(goal)
    },
    deleteClicked (i) {
      this.goals.splice(i, 1)
    },
    scorerSelected (e, i) {
      const scorer = e.target.value

      this.goals[i].goal = scorer
    },
    assisterSelected (e, i) {
      const assister = e.target.value

      this.goals[i].assist = assister
    },
    createClicked () {
      const opponent = this.opponent
      const result = this.result
      const ground = this.ground
      const date = this.date
      const tournament = this.tournament
      const players = this.players
      const subs = this.subs
      const goals = this.goals

      const data = {
        opponent,
        result,
        ground,
        date,
        tournament,
        players,
        subs,
        goals
      }

      axios({
        method: 'post',
        url: 'http://localhost:8000/match/create',
        data
      }).then((res) => {
        this.$router.push('/adminMatches')
      }).catch((err) => {
        alert(err)
      })
    }
  }
}
</script>

<style scoped>
  .title {
    font-weight: bold;
    font-size: 1.1rem;
    margin-bottom: 0.25rem;
    margin-top: 1rem;
  }

  .player {
    display: inline-block;
    width: 20rem;
  }

  .player-inner {
    display: flex;
    align-items: center;
  }

  .player-name {
    display: inline-block;
    white-space: nowrap;
    overflow-x: hidden;
    text-overflow: ellipsis;
  }

  .add {
    font-weight: normal;
    font-size: 1rem;
    color: #00a000;
    margin-left: 1rem;
  }

  .add:hover {
    cursor: pointer;
  }

  .goal {
    display: flex;
    align-items: center;
  }

  .goal select {
    width: 7rem;
    margin: 0rem 1rem;
    white-space: pre-wrap;
  }

  .delete {
    color: #f00000;
    cursor: pointer;
  }

  input {
    font-size: 1rem;
    margin-right: 1rem;
  }

  .create {
    display: inline-block;
    padding: 0.5rem 4rem;
    margin-top: 3rem;
    color: white;
    background-color: #00a000;
    border-radius: 5px;
    cursor: pointer;
  }
</style>
