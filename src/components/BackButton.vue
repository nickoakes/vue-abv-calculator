<template>
  <v-container>
      <v-row>
            <v-col>
                <v-btn 
                    v-on:click="back"
                    elevation="2"
                    class="orange"
                    text
                    rounded>
                    <v-icon
                        color="white">
                        mdi-arrow-left-bold
                    </v-icon>
                    Back
                </v-btn>
            </v-col>
      </v-row>
  </v-container>
</template>

<script>
import EventBus from '../../event-bus';

export default {
  name: 'BackButton',
  data() {
    return {
      
    }
  },
  props: ['stage'],
  methods: {
      back () {
        EventBus.$emit('changeStep', this.stage -1);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
