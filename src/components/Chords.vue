<template>
  <div class="selected-chords">
    <ul v-if ="selectedList" class="chord-box"><li v-for="chord in selectedList" :key="chord" class="chord-text">{{chord}}</li></ul>
  </div>
</template>

<script>
export default {

  props: {
    selectedList: Array,
    numberOfChords: null,
  },
}
</script>

<style>
.selected-chords {
  flex: 1; /* Split equally */
  display: flex;
  align-items: center;
  justify-content: center;
  width:100vw;
  margin: 10vh;
  margin-bottom: 15vh;
}

.chord-box {
  height:100%;
  width: 100%;
  display: grid;
  grid-auto-flow: row;
  grid-template-columns: 1fr 1fr;
  padding: 0;
}

.chord-text{
  font-size: 11em;
  font-weight: bolder;
  list-style-type: none;
  display: block;
  margin: auto;
}
</style>