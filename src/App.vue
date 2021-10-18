<template>
  <v-app>
    <div id="app" align="center">
      <Summary v-show='this.currentStage > 2' :stage='this.currentStage' :drinkData='this.drinkData' :icon='this.defaultSugar.find(x => x.type == drinkData.type).icon' />
      <Home v-show="this.currentStage == 1" />
      <DrinkType v-show="this.currentStage == 2" />
      <DefaultSugar :defaultSugar='defaultSugar.find(x => x.type == drinkData.type)' v-show="this.currentStage == 3" mass="this.defaultSugar[this.drinkData.type]" />
      <AdditionalSugar v-show="this.currentStage == 4" />
      <TotalVolume v-show="this.currentStage == 5" />
      <Result :drinkType='this.drinkData.type' :result='this.result' v-show="this.currentStage == 6" />
    </div>
  </v-app>
</template>

<script>
import Summary from './components/Summary.vue';
import Home from './components/Home.vue';
import DrinkType from './components/DrinkType.vue';
import DefaultSugar from './components/DefaultSugar.vue';
import AdditionalSugar from './components/AdditionalSugar.vue';
import TotalVolume from './components/TotalVolume.vue';
import Result from './components/Result.vue';

import EventBus from '../event-bus.js';

export default {
  name: 'App',
  data() {
    return {
      currentStage: 1,
      drinkData: {
        type: 'Beer',
        totalHoney: 0,
        baseSugar: 0,
        additionalSugar: 0,
        totalVolume: 0
      },
      defaultSugar: [
        {
          type: 'Beer',
          icon: 'mdi-glass-mug-variant',
          sugarContent: 517.7,
          units: 'grams per one standard 40-pint beer kit'
        },
        {
          type: 'Wine',
          icon: 'mdi-glass-wine',
          sugarContent: 160,
          units: 'grams per 1000ml of red grape juice'
        },
        {
          type: 'Cider',
          icon: 'mdi-food-apple',
          sugarContent: 100,
          units: 'grams per 1000ml of apple juice'
        },
        {
          type: 'Mead',
          icon: 'mdi-beehive-outline',
          sugarContent: 81,
          units: 'grams per 100g of clear honey'
        }
      ],
      constants: {
        sucroseMolarMass: 342,
        ethanolMolarMass: 46,
        ethanolDensity: 1.12669
      },
      result: 0
    }
  },
  methods: {
    changeStep (newStage) {
      this.currentStage = newStage;
    },
    updateDrinkData (key, value) {
      this.drinkData[key] = value;
    },
    calculateResult () {
      let totalSugarMass;

      switch(this.drinkData.type) {
        case 'Beer':
          totalSugarMass = this.drinkData.baseSugar + this.drinkData.additionalSugar;
          break;
        case 'Wine':
        case 'Cider':
          totalSugarMass = this.drinkData.baseSugar * this.drinkData.totalVolume + this.drinkData.additionalSugar;
          break;
        case 'Mead':
          totalSugarMass = (this.drinkData.totalHoney / 100) * this.drinkData.baseSugar + this.drinkData.additionalSugar;
          break;
      }

      const sucroseMoles = totalSugarMass / this.constants.sucroseMolarMass;
      const totalVolume = this.drinkData.totalVolume * 1000;

      //C12H22O11 + H2O => 2C6H12O6
      const glucoseMoles = sucroseMoles * 2;

      //C6H12O6 => 2C2H6O + 2CO2
      const ethanolMoles = glucoseMoles * 2;

      const ethanolMass = ethanolMoles * this.constants.ethanolMolarMass;

      const ethanolVolume = ethanolMass * this.constants.ethanolDensity;

      this.result = Math.round((ethanolVolume / totalVolume) * 1000) / 10;
    },
    restoreDefaults () {
      this.currentStage = 1;
      this.drinkData = {
        type: 'Beer',
        totalHoney: 0,
        baseSugar: 0,
        additionalSugar: 0,
        totalVolume: 0
      };
      this.selected = 'Beer';
      this.result = 0;
    }
  },
  mounted () {
      EventBus.$on('changeStep', (newStep) => {
        this.changeStep(newStep);
      });

      EventBus.$on('drinkDataUpdate', (data) => {
        this.updateDrinkData(data.key, data.value);
      });

      EventBus.$on('drinkDataComplete', () => {
        this.calculateResult();
        this.currentStage = 6;
      });

      EventBus.$on('reset', () => {
        this.restoreDefaults();
      });
  },
  components: {
    Summary,
    Home,
    DrinkType,
    DefaultSugar,
    AdditionalSugar,
    TotalVolume,
    Result
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
}

button.v-btn span {
  color: #FFF;
  font-weight: 700;
}

.v-application--wrap {
  justify-content: center;
}
</style>
