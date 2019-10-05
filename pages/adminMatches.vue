<template>
  <div
    class="admin-matches">
    <div
      class="match"
      v-for="(match, i) in data"
      :key="i"
      @click="matchClicked(i)">
      <div
        class="logo-wrapper">
        <img
          class="logo"
          :src="`http://localhost:8000/asset/${match.doc._id}?asset=opponentLogo`">
      </div>
      <div
        class="title">
        <span>{{ match.doc.opponent.title }} - </span>
        <span
          class="ground">{{ match.doc.ground.type }}</span>
      </div>
      <div
        class="result">
        <span
          v-if="match.doc.result !== 'pending'">
          {{ match.doc.goals.length }} - {{ match.doc.opponent.goals }}</span>
        <span>{{ match.doc.result }}</span>
      </div>
      <div>{{ formatDate(new Date(match.doc.date)) }}</div>
    </div>
    <div
      class="create"
      @click="createClicked">Create Match +</div>
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
      url: 'http://localhost:8000/match/all'
    }).then((res) => {
      const rows = res.data.rows

      rows.sort((rowA, rowB) => {
        return new Date(rowA.doc.date) - new Date(rowB.doc.date)
      })

      this.data = rows
    })
  },
  methods: {
    formatDate (date) {
      return dateFormat(date, 'mmmm dS, yyyy')
    },
    createClicked () {
      this.$router.push('/adminCreateMatch')
    },
    matchClicked (i) {
      const match = this.data[i]
      const id = match.doc._id

      this.$router.push('/adminCreateMatch/' + id)
    }
  }
}
</script>

<style scoped>
  .match,
  .create {
    display: inline-block;
    width: 16rem;
    text-align: center;
    padding: 1rem;
    cursor: pointer;
    transition: all 0.1s;
  }

  .match:hover,
  .create:hover {
    transform: scale(1.1);
  }

  .logo-wrapper {
    height: 3.5rem;
    margin-bottom: 0.5rem;
  }

  .logo {
    height: 100%;
  }

  .title {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow-x: hidden;
  }

  .ground,
  .result {
    text-transform: capitalize;
  }
</style>
