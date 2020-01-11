<template>
  <div id="app">
    <img alt="Vue logo" width=30% src="./assets/logo.png">
    <h2>˗ˋˏ ♡ ˎˊ˗ 오늘의 날씨에 따라 멋진 스타일링을 해보세요 ˗ˋˏ ♡ ˎˊ˗ </h2>
    <Weather />
    <Recommendation />
  </div>
</template>

<script>
import Weather from './components/Weather.vue'
import Recommendation from './components/Recommendation.vue'

export default {
  name: 'app',
  components: {
    Weather,
    Recommendation
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
