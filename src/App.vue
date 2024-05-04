<script>
import {defineComponent} from "vue";
import SelectionList from "@/components/ChordsList.vue";
import Timer from './components/Timer.vue';
import Chords from "@/components/Chords.vue";
import metronome from "@/components/Metronome.vue";

export default
defineComponent({
  components: {Chords, Timer, SelectionList, metronome},
  data() {
    return{
      userTimer:60,
      timerLength:60,
      tempoChange:100,
      selectedList: null,
      isMetronomeActive: false,
      tempo:100,
    };
  },
  refs:{timerInput:null, setTempo:null},
  methods: {
    changeMetronomeState(){
      this.isMetronomeActive = !this.isMetronomeActive;
    },
    setTempo(){
      this.tempo=this.tempoChange
    },
    timeChange() {
      this.timerLength=this.userTimer; // Emit event with new length
    },
    handleSelectionChanged(selectedChords){
      this.selectedList = selectedChords;
      console.log('selected chords are ', selectedChords)
    }
  }
})
</script>

<template>
  <h3 class="simple-header">Guitar-Chord-Practice</h3>
  <div id="app" class="site-container">
    <div class="top-block">
      <div class="timer-container">
        <Timer ref="timer" :countdownTime="timerLength" :is-metronome-active="isMetronomeActive" :tempo="tempo"/>
      </div>
      <chords :selected-list="selectedList"/>
    </div>
    <div class="bottom-block">
      <h2>Settings</h2>
      <div class="settings-container">
        <label for="userTimer">Timer Length (seconds):</label>
        <input type="number" id="userTimer" v-model="userTimer" ref="timerInput" min="1"/>
        <button @click="timeChange"> Change Timer </button>
        <h1>metronome</h1>
        <button @click="changeMetronomeState">metronome on or off</button>
        <h1>{{isMetronomeActive}}</h1>
        <input type="number" id="tempoChange" v-model="tempoChange" ref="setTempo" min="100"/>
        <button @click="setTempo">Change Tempo</button>
        <SelectionList @selectionChanged="handleSelectionChanged"/>
      </div>
    </div>
  </div>
</template>

<style scoped>
.simple-header{
  position: absolute;
  width: 100%;
  height: 100%;
  top:1vh;
  left:2vw;
}

.site-container {
  display:flex;
  flex-direction: column;
}

.top-block {
  display: flex;
  width: 100%; /* Full screen width */
  height: 100vh; /* Half viewport height */
}

.timer-container{
  flex: 1; /* Split equally */
  display: flex;
  align-items: center;
  justify-content: center;
  width:100vw;
  margin: 10vh;
}

.bottom-block {
  padding: 20px;
  border-top: 1px solid #ddd;
}

.settings-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 10px;
}

label {
  margin-bottom: 5px;
}

input[type="number"] {
  padding: 5px;
  border: 1px solid #ddd;
}
</style>