<template>
  <div id="app">
    <HomePage />
  </div>
</template>

<script>
import HomePage from './components/Homepage'

export default {
  name: 'App',
  components: {
    HomePage
  }
}
</script>

<style lang="css">
    @import './css/bootstrap-grid.min.css';
    @import './css/styles.css';
</style>
