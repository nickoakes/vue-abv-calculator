<template>
  <v-container>
      <v-row>
            <v-col>
                <h2>What's the total volume?</h2>
            </v-col>
      </v-row>
      <v-row>
            <v-col cols="3"></v-col>
            <v-col cols="6">
                <v-text-field
                    v-on:keyup="toggleContinue"
                    label="Total volume"
                    suffix="litres"
                    v-model.number="totalVolume">
                </v-text-field>            
            </v-col>
      </v-row>
      <v-row v-show="allowContinue">
            <v-col>
                <v-btn 
                    v-on:click="select"
                    elevation="2"
                    color="accent"
                    rounded>
                    Continue
                </v-btn>
            </v-col>
        </v-row>
  </v-container>
</template>

<script>
import EventBus from '../../event-bus';

export default {
  name: 'TotalVolume',
  data() {
      return {
          stage: 5,
          totalVolume: 0,
          allowContinue: false
    }
  },
  props: ['defaultSugar'],
  methods: {
      toggleContinue () {
          if(!isNaN(this.totalVolume) && this.totalVolume > 0) {
              this.allowContinue = true;
          } else {
              this.allowContinue = false;
          }
      },
      select () {
        EventBus.$emit('drinkDataUpdate',{ 'key': 'totalVolume', 'value': this.totalVolume });
        EventBus.$emit('drinkDataComplete');
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
