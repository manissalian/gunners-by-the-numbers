<template>
  <div
    class="home">
    <div
      style="flex: 1">
      <next-match/>
      <div
        class="separator"/>
      <div
        class="title">Top Scorers</div>
      <podium/>
      <div
        class="separator"/>
      <div
        class="title">Top Assists</div>
      <podium/>
    </div>
    <mini-league-table/>
  </div>
</template>

<script>
import NextMatch from '@/components/nextMatch'
import Podium from '@/components/podium'
import MiniLeagueTable from '@/components/miniLeagueTable'

export default {
  components: {
    NextMatch,
    Podium,
    MiniLeagueTable
  }
}
</script>

<style scoped>
  .home {
    display: flex;
  }

  .title {
    text-align: center;
    font-size: 1.5rem;
    margin-top: 4rem;
  }

  .separator {
    height: 1px;
    background-color: rgba(0, 0, 0, 0.2);
    margin-top: 4rem;
    max-width: 768px;
    margin-left: auto;
    margin-right: auto;
  }
</style>
