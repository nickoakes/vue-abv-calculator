<template>
  <v-container>
      <v-row>
            <v-col>
                <h2>Have you added any additional sugar?</h2>
            </v-col>
      </v-row>
      <v-row v-show="!additionalSugarAdded">
          <v-col cols="3"></v-col>
            <v-col cols="3">
                <v-btn 
                    v-on:click="addAdditionalSugar"
                    elevation="2"
                    color="accent"
                    rounded>
                    Yes
                </v-btn>            
            </v-col>
            <v-col cols="3">
                <v-btn 
                    v-on:click="select"
                    elevation="2"
                    color="accent"
                    rounded>
                    No
                </v-btn>
            </v-col>
            <v-col cols="3"></v-col>
      </v-row>
      <v-row v-show="additionalSugarAdded">
            <v-col cols="3"></v-col>
            <v-col cols="6">
                <v-text-field
                    v-on:keyup="toggleContinue"
                    label="How much did you add?"
                    suffix="grams"
                    v-model.number="additionalSugar">
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
  name: 'AdditionalSugar',
  data() {
      return {
          stage: 4,
          additionalSugarAdded: false,
          additionalSugar: 0,
          allowContinue: false
    }
  },
  props: ['defaultSugar'],
  methods: {
      addAdditionalSugar () {
          this.additionalSugarAdded = true;
      },
      toggleContinue () {
          if(!isNaN(this.additionalSugar) && this.additionalSugar > 0) {
              this.allowContinue = true;
          } else {
              this.allowContinue = false;
          }
      },
      select () {
        EventBus.$emit('drinkDataUpdate',{ 'key': 'additionalSugar', 'value': this.additionalSugar });
        EventBus.$emit('changeStep', this.stage + 1);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
