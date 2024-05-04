<template>
  <div class="metronome">
    <button @click="togglePlay">
      <span v-if="isPlaying">Stop</span>
      <span v-else>Start</span>
    </button>
    <div class="tempo">
      Tempo: {{ tempo }} BPM
      <input type="range" min="40" max="200" v-model="tempo" @change="setTempo">
    </div>
  </div>
</template>

<script>
export default {
  props:{
    isPlaying:Boolean,
    tempo:Number,
  },
  data() {
    return {
      isPlaying: this.isPlaying,
      tempo: this.tempo,
      intervalId: null,
    }
  },
  methods: {
    togglePlay() {
      if (this.isPlaying) {
        this.stopMetronome();
      } else {
        this.startMetronome();
      }
    },
    startMetronome() {
      this.isPlaying = true;
      this.intervalId = setInterval(this.playClick, 1000 / this.tempo * 60);
    },
    stopMetronome() {
      this.isPlaying = false;
      clearInterval(this.intervalId);
      this.intervalId = null;
    },
    playClick() {
      // Simulate a metronome click sound using a short audio element
      const audio = new Audio();
      audio.src = "/Metronome_sounds/Perc_Can_hi.wav"; // Short click sound (base64 encoded)
      audio.play();
    },
    setTempo(newTempo) {
      this.tempo = newTempo;
      if (this.isPlaying) {
        this.stopMetronome();
        this.startMetronome();
      }
    }
  },
  destroyed() {
    this.stopMetronome();
  }
}
</script>

<style scoped>
.metronome {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}

.tempo {
  display: flex;
  align-items: center;
}
</style>