<template>
  <div class="state-control">
    <h3>{{ state }}</h3>
    <div>Population: {{ pop }} ({{ electoral }} EV)</div>

    <div>
      <input type="radio" :id="`wta-${_uid}`" value="wta" v-model="mode">
      <label :for="`wta-${_uid}`">Winner Take All</label>
      <br>
      <input type="radio" :id="`prop-${_uid}`" value="prop" v-model="mode">
      <label :for="`prop-${_uid}`">Proportional</label>
      <br>
      <input type="radio" :id="`npvic-${_uid}`" value="npvic" v-model="mode">
      <label :for="`npvic-${_uid}`">NPVIC</label>
    </div>

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
      this.$emit('votes', this.state, votes, this.mode);
      return votes;
    },
  },
  data () {
    return {
      percent: 0,
      mode: 'wta',
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
