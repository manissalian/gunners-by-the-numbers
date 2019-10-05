<template>
  <div
    class="player-profile">
    <div
      class="top">
      <img
        :src="`http://localhost:8000/asset/f65446f7594ca97e224662a3bf0059c8?asset=player_big.png`">
      <div
        class="info">
        <div
          class="title">14. Pierre Emerick Aubameyang</div>
        <table>
          <tr>
            <th>Games:</th>
            <td>30</td>
          </tr>
          <tr>
            <th>Wins:</th>
            <td>20</td>
          </tr>
          <tr>
            <th>Draws:</th>
            <td>7</td>
          </tr>
          <tr>
            <th>Losses:</th>
            <td>3</td>
          </tr>
          <tr>
            <th>Goals:</th>
            <td>22</td>
          </tr>
          <tr>
            <th>Assists:</th>
            <td>11</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
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
