<template>
  <div id="app">
    <state-control v-for="state in states"
      :key="state.name"
      :state="state.name"
      :electoral="state.electoral"
      :pop="state.pop" />
  </div>
</template>

<script>
import StateControl from './components/StateControl.vue'

export default {
  name: 'app',
  components: {
    StateControl
  },
  data () {
    return {
      states: [
        {
          name: 'CA',
          pop: 12,
          electoral: 4,
        },
        {
          name: 'IL',
          pop: 23,
          electoral: 8,
        }
      ]
    };
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
}
</style>
