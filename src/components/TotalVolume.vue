<template>
  <v-container>
      <BackButton :stage='this.stage' />
      <v-row>
            <v-col>
                <h2>What's the total volume?</h2>
            </v-col>
      </v-row>
      <v-row>
            <v-col cols="3"></v-col>
            <v-col cols="6">
                <v-form class="d-flex flex-grow-1" v-on:submit="select">
                    <v-text-field
                        v-on:keyup="toggleContinue"
                        label="Total volume"
                        suffix="litres"
                        v-model.number="totalVolume"
                        pattern="\d*"
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
      restoreDefaults () {
          this.totalVolume = 0;
          this.allowContinue = false;
      },
      toggleContinue () {
          if(!isNaN(this.totalVolume) && this.totalVolume > 0) {
              this.allowContinue = true;
          } else {
              this.allowContinue = false;
          }
      },
      select (e) {
        if (e != null) e.preventDefault();
        EventBus.$emit('drinkDataUpdate',{ 'key': 'totalVolume', 'value': this.totalVolume });
        EventBus.$emit('drinkDataComplete');
      },
      clearInput () {
          this.totalVolume = this.totalVolume == 0 ? '' : this.totalVolume;
      },
      restoreInput () {
          this.totalVolume = this.totalVolume == '' ? 0 : this.totalVolume;
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
