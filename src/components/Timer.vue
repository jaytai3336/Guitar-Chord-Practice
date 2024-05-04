<template>
  <div class="timer">
    <div class="timerNo">{{remainingTime}}</div>
    <p v-if="!paused" class="timer-text">seconds left </p>
    <p v-else class="timer-text">Paused</p>
    <div class="button-container">
      <button @click="toggleTimer"> {{ inResetState ? 'Start' : (paused ? 'Resume' : 'Stop') }} </button>
      <div>/</div>
      <button @click="resetTimer">Reset</button>
    </div>
    <audio ref="metronomeAudio" src="/Metronome_sounds/Perc_Can_hi.wav"></audio>
  </div>
</template>

<script>
export default {
  props: {
    countdownTime: {
      type: Number,
      default: 10,
    },
    isMetronomeActive: Boolean,
    tempo: Number,
  },
  data() {
    return {
      remainingTime: this.countdownTime, // Initial value
      intervalId: null,
      metronomeIntervalId:null,
      paused: false,
      inResetState: true, // Added: Flag for reset state
    };
  },
  mounted() {
    // No timer start on mount if in reset state
  },
  beforeDestroy() {
    clearInterval(this.intervalId); // Stop interval on component destruction
    clearInterval(this.metronomeIntervalId);
  },
  methods: {
    decrementTime() {
      if (this.remainingTime > 0 && !this.paused) {
        this.remainingTime--;
        this.playMetronomeClick(); // Play metronome click on each decrement
      } else if (this.remainingTime === 0) {
        this.stopTimer();
      }
    },
    toggleTimer() {
      if (this.inResetState) {
        // Start timer at 1 second interval
        this.intervalId = setInterval(this.decrementTime, 1000);
        // Start metronome based on isMetronomeActive prop (if provided)
        if (this.isMetronomeActive) {
          this.startMetronome();
        }
        this.inResetState = false;
      } else {
        this.paused = !this.paused;
        if (this.paused) {
          clearInterval(this.intervalId);
          if (this.isMetronomeActive) {
            clearInterval(this.metronomeIntervalId);
          }
        } else {
          // Restart timer interval when resuming from pause
          clearInterval(this.intervalId);
          this.intervalId = setInterval(this.decrementTime, 1000);
          if (this.isMetronomeActive) {
            this.startMetronome();
          }
        }
      }
    },
    startMetronome() {
      if (this.tempo) {
        const metronomeInterval = (this.countdownTime * 1000) / (this.tempo * 60);
        this.metronomeIntervalId = setInterval(this.playMetronomeClick, metronomeInterval);
      } else {
        console.warn("Tempo prop is required for metronome functionality.");
      }
    },
    stopTimer() {
      clearInterval(this.intervalId);
      this.paused = true;
    },
    resetTimer() {
      this.stopTimer();
      this.remainingTime = this.countdownTime;
      this.paused = false;
      this.inResetState = true; // Reset state on reset button click
    },

    playMetronomeClick() {
      if (this.isMetronomeActive) {
        this.$refs.metronomeAudio.play(); // Play metronome sound
      }
    },

  },
};
</script>

<style scoped>
.timer {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.timerNo {
  font-size: 30vh;
  font-weight: bolder;
  margin-bottom: -9vh;
  margin-top: -10vh;

}

.timer-text{
  margin-bottom: 2vh;
  margin-left: 14vw;
  font-style: italic;

}

.display {
  font-size: 24px;
  margin-bottom: 10px;
}

.controls {
  display: flex;
  justify-content: space-between;
}

button {
  padding: 5px 10px;
  border-style: none;
  cursor: pointer;
  background-color: white;
}

button:disabled {
  opacity: 0.5;
  cursor: default;
}

.button-container {
  padding-top: 5vh;
  display: grid;
  grid-auto-flow: column;
  grid-column-gap: 1vw;
}

</style>