<template>
  <div id="app">
    <web-nav></web-nav>
    <router-view/>
    <footer class="copyright">© All rights reserved.</footer>
  </div>
</template>

<script>
import webNav from '@/base/nav'
export default {
  name: 'App',
  components: {
    webNav
  }
}
</script>

<style>

</style>
