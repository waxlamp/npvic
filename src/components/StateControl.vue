<template>
  <div class="state-control">
    <h3>{{ state }}</h3>
    <div>Population: {{ pop }} ({{ electoral }} EV)</div>
    <input type="range" min="0" max="1" step="0.01" v-model="percent">
    <div>Votes: {{ votes }}</div>
  </div>
</template>

<script>
export default {
  props: {
    state: String,
    pop: Number,
    electoral: Number,
  },
  computed: {
    votes () {
      const votes = Math.floor(this.percent * this.pop + 0.5);
      this.$emit('votes', this.state, votes);
      return votes;
    },
  },
  data () {
    return {
      percent: 0
    };
  }
};
</script>

<style scoped>
div.state-control {
  border: solid;
  min-width: 120px;
}
</style>
