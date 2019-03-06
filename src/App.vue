<template>
  <div>
    <div id="app">
      <state-control v-for="state in states"
        :key="state.name"
        :state="state.name"
        :electoral="state.electoral"
        :pop="state.pop"
        @votes="updateResult" />
    </div>
    <div>
      Winner-take-all: {{ winnerTakeAll }}
    </div>
    <div>
      Proportional: {{ proportional }}
    </div>
    <div>
      NPVIC: {{ npvic }}
    </div>
  </div>
</template>

<script>
import StateControl from './components/StateControl.vue'

const sum = (acc, x) => acc + x;

export default {
  name: 'app',
  components: {
    StateControl
  },
  computed: {
    winnerTakeAll () {
      return Object.values(this.states)
        .map(s => s.votes > 0.5 * s.pop ? s.electoral : 0)
        .reduce(sum);
    },
    proportional () {
      return Object.values(this.states)
        .map(s => Math.floor((s.votes / s.pop) * s.electoral))
        .reduce(sum);
    },
    npvic () {
      const votes = Object.values(this.states)
        .map(s => s.votes)
        .reduce(sum);

      const all = Object.values(this.states)
        .map(s => s.pop)
        .reduce(sum);

      return Object.values(this.states)
        .map(s => votes / all > 0.5 ? s.electoral : 0)
        .reduce(sum);
    },
  },
  methods: {
    updateResult (which, votes) {
      this.states[which].votes = votes;
    }
  },
  data () {
    return {
      states: {
        CA: {
          name: 'CA',
          pop: 12,
          electoral: 4,
          votes: 0,
        },
        IL: {
          name: 'IL',
          pop: 23,
          electoral: 8,
          votes: 0,
        }
      },
      // votes: {
        // CA: 0,
        // IL: 0
      // },
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
