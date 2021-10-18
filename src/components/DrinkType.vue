<template>
  <v-container>
      <v-row>
            <v-col>
                <h2>What have you made?</h2>
            </v-col>
      </v-row>
      <v-row>
          <v-col>
            <v-radio-group row v-model="selected" class="d-none d-md-flex">
                <v-radio
                    v-for="n in this.drinks"
                    :key="`drink-${n}`"
                    :label="n"
                    :value="n"
                >
                </v-radio>
            </v-radio-group>
            <v-radio-group column v-model="selected" class="d-flex d-md-none">
                <v-radio
                    v-for="n in this.drinks"
                    :key="`drink-${n}`"
                    :label="n"
                    :value="n"
                    class="mt-5"
                ></v-radio>
            </v-radio-group>
          </v-col>
      </v-row>
      <v-row v-show="this.selected == 'Mead'">
            <v-col>
                <h2>How much honey did you use?</h2>
            </v-col>
      </v-row>
      <v-row v-show="this.selected == 'Mead'">
            <v-col cols="3"></v-col>
            <v-col cols="6">
                <v-form class="d-flex flex-grow-1" v-on:submit="select">
                    <v-text-field
                        label="Mass of honey"
                        suffix="grams"
                        v-model.number="totalHoney"
                        pattern="\d*">
                    </v-text-field>
                </v-form>
            </v-col>
      </v-row>
      <v-row v-show="this.selected != 'Mead' || this.totalHoney > 0">
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
import EventBus from '../../event-bus'
export default {
  name: 'DrinkType',
  data() {
      return {
          stage: 2,
          drinks: [
              'Beer',
              'Wine',
              'Cider',
              'Mead'
          ],
          selected: 'Beer',
          totalHoney: ''
    }
  },
  methods: {
      restoreDefaults () {
          this.selected = 'Beer';
          this.totalHoney = '';
      },
      select (e) {
          if(e != null) e.preventDefault();
          if(!(this.selected == 'Mead' && this.totalHoney == 0)) {
            EventBus.$emit('drinkDataUpdate',{ 'key': 'type', 'value': this.selected });
            EventBus.$emit('drinkDataUpdate',{ 'key': 'totalHoney', 'value': this.totalHoney });
            EventBus.$emit('changeStep', this.stage + 1);
          }
      }
  },
  props: ['drinkData'],
  mounted(){
      EventBus.$on('reset', () => {
          this.restoreDefaults();
      });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.v-input--radio-group__input {
    justify-content: center !important;
}
</style>
