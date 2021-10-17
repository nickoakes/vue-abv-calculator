<template>
  <div id="app">
    <Home />
    <DrinkType @start="start=true" />
  </div>
</template>

<script>
import Home from './components/Home.vue'
import DrinkType from './components/DrinkType.vue'

export default {
  name: 'App',
  data() {
    return {
      currentStage: 'Home'
    }
  },
  components: {
    Home,
    DrinkType
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.proceed-button {
  cursor: pointer;
  font-size: 16px;
  font-weight: 700;
  border-radius: 5px;
}
</style>
