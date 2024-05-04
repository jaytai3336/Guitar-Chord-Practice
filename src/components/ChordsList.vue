<template>
  <div class="selection-list">
    <h2>Guitar Chords</h2>
    <ul>
      <li v-for="chord in availableChords" :key="chord">
        <input type="checkbox" :id="chord" v-model="selectedChords" :value="chord" />
        <label :for="chord">{{ chord }}</label>
      </li>
    </ul>
    <button @click="$emit('selectionChanged', selectedChords.slice())">Get Selected</button>
  </div>
</template>

<script>
export default {
  props: {
    availableChords: Array, // Receive available chords as props
  }, data() {
    return {
      selectedChords: [], // Array to track selected chords
      availableChords: ['A', 'B', 'C', 'D', 'E', 'G'],
    };
  }, methods: {
    // No methods needed in this case
  },};
</script>

<style scoped>

</style>