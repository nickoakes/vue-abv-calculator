<template>
<v-container>
    <v-container class="d-none d-md-flex">
        <v-row>
            <v-col>
                <v-icon color="green">
                    {{ icon }}
                </v-icon>
                <p class="green--text">{{ drinkData.type }}</p>
            </v-col>
            <v-col v-show='this.stage > 3'>
                <v-icon color="green">
                    mdi-cube-outline
                </v-icon>
                <p class="green--text">{{ drinkData.baseSugar }}g base</p>
            </v-col>
            <v-col v-show='this.stage > 4'>
                <v-icon color="green">
                    mdi-spoon-sugar
                </v-icon>
                <p class="green--text">{{ drinkData.additionalSugar }}g additional</p>
            </v-col>
            <v-col v-show='this.stage > 5'>
                <v-icon color="green">
                    mdi-bucket-outline
                </v-icon>
                <p class="green--text">{{ drinkData.totalVolume }} litres</p>
            </v-col>
        </v-row>
    </v-container>
    <v-container class="d-flex flex-column d-md-none">
        <v-row>
            <v-col>
                <v-icon color="green">
                    {{ icon }}
                </v-icon>
                <p class="green--text">{{ drinkData.type }}</p>
            </v-col>
            <v-col v-show='this.stage > 3'>
                <v-icon color="green">
                    mdi-cube-outline
                </v-icon>
                <p class="green--text">{{ drinkData.baseSugar }}g base</p>
            </v-col>
        </v-row>
        <v-row v-show='this.stage > 4'>
            <v-col v-show='this.stage > 4'>
                <v-icon color="green">
                    mdi-spoon-sugar
                </v-icon>
                <p class="green--text">{{ drinkData.additionalSugar }}g additional</p>
            </v-col>
            <v-col v-show='this.stage > 5'>
                <v-icon color="green">
                    mdi-bucket-outline
                </v-icon>
                <p class="green--text">{{ drinkData.totalVolume }} litres</p>
            </v-col>
        </v-row>
    </v-container>
</v-container>
</template>

<script>

export default {
  name: 'Summary',
  data() {
      return {

    }
  },
  props: ['stage', 'drinkData', 'icon'],
  methods: {

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
