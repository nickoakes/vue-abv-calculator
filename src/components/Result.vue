<template>
  <v-container>
      <v-row>
            <v-col>
                <h2>Your {{drinkType}} is approximately {{result}}% A.B.V.</h2>
            </v-col>
      </v-row>
      <v-row>
            <v-col>
                <v-btn 
                    v-on:click="reset"
                    elevation="2"
                    color="accent"
                    rounded>
                    Reset
                </v-btn>
            </v-col>
        </v-row>
  </v-container>
</template>

<script>
import EventBus from '../../event-bus';

export default {
  name: 'Result',
  data() {
      return {
          stage: 6
    }
  },
  props: ['drinkType', 'result'],
  methods: {
      reset () {
          EventBus.$emit('reset');
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
