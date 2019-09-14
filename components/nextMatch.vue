<template>
  <div
    class="next-match">
    <img
      class="tournament-logo"
      src="@/assets/logos/premier_league.png"/>
    <div
      class="title">Next Match</div>
    <div
      class="date">10 October 2:30PM</div>
    <div
      class="stadium">Emirates Stadium</div>
    <div
      class="row">
      <div
        class="team-wrapper">
        <div
          class="logo-wrapper">
          <img
            class="logo"
            src="@/assets/logos/arsenal.png"/>
        </div>
        <div
          class="team">Arsenal</div>
      </div>
      <div
        class="vs">VS</div>
      <div
        class="team-wrapper">
        <div
          class="logo-wrapper">
          <img
            class="logo"
            src="@/assets/logos/tottenham.png"/>
        </div>
        <div
          class="team">Tottenham</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .next-match {
    position: relative;
    max-width: 768px;
    margin-left: auto;
    margin-right: auto;
  }

  .tournament-logo {
    position: absolute;
    width: 13%;
    min-width: 5rem;
    margin-top: 0.75rem;
  }

  .title {
    text-align: center;
    font-size: 1.5rem;
  }

  .date {
    text-align: center;
  }

  .stadium {
    text-align: center;
    font-size: 0.9rem;
    color: #696969;
  }

  .row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    margin-top: 0.5rem;
  }

  .team-wrapper {
    text-align: center;
  }

  .team {
    margin-top: 0.5rem;
  }

  .logo-wrapper {
    width: 7rem;
    height: 7rem;
    text-align: center;
  }

  .logo {
    height: 100%;
  }

  .vs {
    font-size: 1.25rem;
  }
</style>
