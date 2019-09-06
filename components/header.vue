<template>
  <div
    class="header">
    <div
      class="inner">
      <logo />
    </div>
  </div>
</template>

<script>
import Logo from '@/components/logo'

export default {
  components: {
    Logo
  }
}
</script>

<style scoped>
  .header {
    background-color: #f00000;
  }

  .inner {
    display: flex;
    align-items: center;
    max-width: 1366px;
    height: 90px;
    padding-left: 2rem;
    padding-right: 2rem;
    margin-left: auto;
    margin-right: auto;
    box-sizing: border-box;
  }

  .logo {
    display: inline-block;
    line-height: 1.5rem;
    text-align: right;
    font-weight: bold;
    font-style: italic;
    color: white;
  }
</style>
