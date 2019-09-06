<template>
  <div
    class="layout">
    <headerr />
    <nuxt
      class="container"
    />
  </div>
</template>

<script>
import headerr from '@/components/header'

export default {
  components: {
    headerr
  }
}
</script>

<style scoped>
  .layout {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }

  .container {
    flex: 1;
    width: 100%;
    max-width: 1366px;
    padding-top: 1rem;
    padding-left: 2rem;
    padding-right: 2rem;
    margin-left: auto;
    margin-right: auto;
    box-sizing: border-box;
  }
</style>

<style>
  html {
    font-size: 12px;
  }

  * {
    margin: 0;
  }

  body {
    font-family: MartelRegular;
    background-color: #dddddd;
  }

  @font-face {
    font-family: 'MartelRegular';
    src: url('../assets/fonts/martel_regular.woff') format('woff');
  }

  @media screen and (min-width: 768px) {
    html {
      font-size: 16px;
    }
  }
</style>
