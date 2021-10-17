<template>
  <div class="stage-wrapper" v-show="this.show">
    <h1>What have you made?</h1>
  </div>
</template>

<script>
import EventBus from '../../event-bus'
export default {
  name: 'DrinkType',
  data() {
      return {
          show: false
    }
  },
  methods: {

  },
  mounted(){
      EventBus.$on('start', () => {
          this.show = true;
      });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
