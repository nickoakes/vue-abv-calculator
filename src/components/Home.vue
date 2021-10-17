<template>
  <div class="stage-wrapper" v-show="!this.started">
    <h1>Use this calculator to work out the approximate A.B.V. of home-brewed drinks.</h1>
    <input 
        class="proceed-button" 
        type="button" 
        value="Start"
        v-on:click="start" />
  </div>
</template>

<script>
import EventBus from '../../event-bus.js';

export default {
  name: 'Home',
  data() {
      return {
          started: false
    }
  },
  methods: {
      start() {
          this.started = true;
          EventBus.$emit('start', true);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
