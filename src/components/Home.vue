<template>
  <v-container>
        <v-row>
            <v-col>
                <h2>Use this calculator to work out the approximate A.B.V. of home-brewed drinks.</h2>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-btn 
                    v-on:click="start"
                    elevation="2"
                    color="accent"
                    rounded>
                    Start
                </v-btn>
            </v-col>
        </v-row>
  </v-container>
</template>

<script>
import EventBus from '../../event-bus.js';

export default {
  name: 'Home',
  data() {
      return {
          stage: 1
    }
  },
  methods: {
      start() {
          EventBus.$emit('changeStep', this.stage + 1);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
