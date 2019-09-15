<template>
  <div
    class="mini-league-table">
    <img
      class="title"
      src="@/assets/logos/tournaments/premierLeague.png"/>
    <table>
      <tr
        v-for="(t, i) in data"
        :key="i"
        class="team">
        <td
          class="position">{{ i + 1 }}</td>
        <td>
          <img
            src="@/assets/logos/teams/arsenal.svg"
            class="logo"/>
        </td>
        <td
          class="name">Arsenal</td>
        <td
          class="games">38</td>
        <td
          class="diff">+66</td>
        <td
          class="points">96</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      data: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 18, 18, 19]
    }
  }
}
</script>

<style scoped>
  .mini-league-table {
    display: none;
    font-size: 0.8rem;
  }

  .title {
    width: 7rem;
  }

  .logo {
    height: 1.1rem;
    margin-top: 0.5rem;
    padding-left: 0.5rem;
  }

  .name {
    padding-left: 0.5rem;
    padding-right: 1rem;
    margin-left: 0.5rem;
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
