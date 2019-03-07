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
    <div>
      Per-state: {{ tally }}
    </div>
  </div>
</template>

<script>
import StateControl from './components/StateControl.vue'

const sum = (acc, x) => acc + x;

function computeVotes (state, totalVotes, totalPop) {
  let votes;

  switch(state.mode) {
  case 'wta':
    votes = state.votes > 0.5 * state.pop ? state.electoral : 0;
    break;

  case 'prop':
    votes = Math.floor((state.votes / state.pop) * state.electoral);
    break;

  case 'npvic':
    votes = totalVotes / totalPop > 0.5 ? state.electoral : 0;
    break;

  default:
    throw new Error(`impossible tally mode: ${state.mode}`);
  }

  return votes;
}

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
    tally () {
      const votes = Object.values(this.states)
        .map(s => s.votes)
        .reduce(sum);

      const all = Object.values(this.states)
        .map(s => s.pop)
        .reduce(sum);

      return Object.values(this.states)
        .map(s => computeVotes(s, votes, all))
        .reduce(sum);
    },
  },
  methods: {
    updateResult (which, votes, mode) {
      this.states[which].votes = votes;
      this.states[which].mode = mode;
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
          mode: 'wta',
        },
        IL: {
          name: 'IL',
          pop: 23,
          electoral: 8,
          votes: 0,
          mode: 'wta',
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
