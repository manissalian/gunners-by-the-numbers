<template>
  <div
    class="header">
    <div
      class="inner">
      <router-link
        to="/">
        <logo/>
      </router-link>
      <router-link
        v-for="(page, i) in pages"
        :key="i"
        :to="page.path"
        :style="{
          fontWeight: path === page.path ? 'bold' : 'normal',
          borderBottomWidth: path === page.path ? '1px' : '0px'
        }"
        class="btn">
        <div>{{ page.title }}</div>
      </router-link>
    </div>
  </div>
</template>

<script>
import Logo from '@/components/logo'

export default {
  components: {
    Logo
  },
  data () {
    return {
      pages: [
        {
          path: '/players',
          title: 'Players'
        },
        {
          path: '/schedule',
          title: 'Schedule'
        },
        {
          path: '/results',
          title: 'Results'
        },
        {
          path: '/statsTable',
          title: 'Stats Table'
        }
      ]
    }
  },
  computed: {
    path () {
      return this.$route.path
    }
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

  .btn {
    margin-left: 3rem;
    font-size: 1.15rem;
    color: white;
    text-decoration: none;
    border-width: 0px;
    border-color: white;
    border-style: solid;
  }
</style>
