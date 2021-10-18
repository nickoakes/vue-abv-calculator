<template>
  <v-container>
      <BackButton :stage='this.stage' />
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
                <v-form class="d-flex flex-grow-1" v-on:submit="select">
                    <v-text-field
                        v-on:keyup="toggleContinue"
                        label="How much did you add?"
                        suffix="grams"
                        pattern="\d*"
                        v-model.number="additionalSugar"
                        v-on:click="clearInput"
                        v-on:blur="restoreInput">
                    </v-text-field>
                </v-form>
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
import BackButton from './BackButton.vue';

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
      restoreDefaults () {
          this.additionalSugarAdded = false;
          this.additionalSugar = 0;
          this.allowContinue = false;
      },
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
      select (e) {
        if (e != null) e.preventDefault();
        EventBus.$emit('drinkDataUpdate',{ 'key': 'additionalSugar', 'value': this.additionalSugar });
        EventBus.$emit('changeStep', this.stage + 1);
      },
      clearInput () {
          this.additionalSugar = this.additionalSugar == 0 ? '' : this.additionalSugar;
      },
      restoreInput () {
          this.additionalSugar = this.additionalSugar == '' ? 0 : this.additionalSugar;
      }
  },
  mounted () {
      EventBus.$on('reset', () => {
          this.restoreDefaults();
      });
  },
  components: {
      BackButton
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
