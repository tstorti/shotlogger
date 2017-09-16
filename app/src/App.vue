<template>
  <div id="app">
    <h3>{{ msg }}</h3>
    <shot-logger></shot-logger>
    <!-- <shot-chart></shot-chart> -->
  </div>
</template>

<script>

import ShotLogger from './components/ShotLogger.vue';
// import ShotChart from './components/ShotChart.vue';

export default {
  name: 'app',
  components: {
    'shot-logger': ShotLogger,
    // 'shot-chart':ShotChart,
	},
  data () {
    return {
      msg: 'Welcome to Basketball Shot-Logger'
    }
  }
}
</script>

<style>
/* #app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
