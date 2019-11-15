<template>
  <div
    class="results">
    <MatchList
      :matches="data"/>
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

      const filteredRows = rows.filter((row) => {
        return row.doc.result !== 'pending'
      })

      this.data = filteredRows
    })
  }
}
</script>
