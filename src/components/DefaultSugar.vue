<template>
  <v-container>
      <BackButton :stage='this.stage' />
      <v-row>
            <v-col>
                <h2>The default sugar content for {{ defaultSugar.type.toLowerCase() }} is {{ defaultSugar.sugarContent }} {{ defaultSugar.units }}</h2>
            </v-col>
      </v-row>
      <v-row v-show="!defaultNotUsed">
            <v-col>
                <h2>Would you like to use this default amount for the calculation?</h2>
            </v-col>
      </v-row>
      <v-row v-show="!defaultNotUsed">
          <v-col cols="3"></v-col>
            <v-col cols="3">
                <v-btn 
                    v-on:click="useDefault"
                    elevation="2"
                    color="accent"
                    rounded>
                    Yes
                </v-btn>            
            </v-col>
            <v-col cols="3">
                <v-btn 
                    v-on:click="notUseDefault"
                    elevation="2"
                    color="accent"
                    rounded>
                    No
                </v-btn>
            </v-col>
            <v-col cols="3"></v-col>
      </v-row>
      <v-row v-show="defaultNotUsed">
          <v-col class="d-none d-md-flex">
              <v-row>
                <v-col cols="3"></v-col>
                <v-col cols="6">
                    <v-form v-on:submit="select">
                        <v-text-field
                            v-on:keyup="toggleContinue"
                            label="Enter a new base sugar amount"
                            :suffix="this.defaultSugar.units"
                            v-model.number="baseSugar"
                            pattern="\d*">
                        </v-text-field>
                    </v-form>
                </v-col>
              </v-row>
          </v-col>

            <v-col cols="12" class="d-flex d-md-none">
                <v-form class="d-flex flex-grow-1" v-on:submit="select">
                    <v-text-field
                        v-on:keyup="toggleContinue"
                        label="Enter a new base sugar amount"
                        v-model.number="baseSugar"
                        pattern="\d*">
                    </v-text-field>
                </v-form>
            </v-col>
            <v-col class="d-md-none">
                <p>({{this.defaultSugar.units}})</p>
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
  name: 'DefaultSugar',
  data() {
      return {
          stage: 3,
          baseSugar: '',
          defaultNotUsed: false,
          allowContinue: false
    }
  },
  props: ['defaultSugar'],
  methods: {
      restoreDefaults () {
          this.baseSugar = '';
          this.defaultNotUsed = false;
          this.allowContinue = false;
      },
      useDefault () {
        this.baseSugar = this.defaultSugar.sugarContent;
        this.select();
      },
      notUseDefault () {
          this.defaultNotUsed = true;
      },
      toggleContinue () {
          if(!isNaN(this.baseSugar) && this.baseSugar > 0) {
              this.allowContinue = true;
          } else {
              this.allowContinue = false;
          }
      },
      select (e) {
        if(e != null) e.preventDefault();
        if(!isNaN(this.baseSugar) && this.baseSugar > 0) {
            EventBus.$emit('drinkDataUpdate',{ 'key': 'baseSugar', 'value': this.baseSugar });
            EventBus.$emit('changeStep', this.stage + 1);
        }
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
