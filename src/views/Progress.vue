<template>
  <div>
    <h1>PROGRESS MOVES</h1>
    <p>Check your progress move. You roll challenge dice against your progress. <b>Momemtum is ignored</b>.</p>
    <h3>Progress Move:</h3>
    <p>Select one.</p>
    <select v-model="progressMoveSelected" name="progress-moves" id="progress-moves" :disabled="disabled">
      <option v-for="move in movesFilter" :key="move.title" :value="move">{{ move.title }}</option>
    </select>

    <div v-if="this.progressMoveSelected != 'none'" class="move-card">
      <h3>{{ this.progressMoveSelected.title }}</h3>

      <h4>Description</h4>

      <p v-html="progressMoveSelected.desc"></p>
    </div>

    <h3>Progress Track:</h3>
    <p><input v-model="progressTrack" type="number" name="progress" id="progress" :disabled="disabled" /></p>
    <div v-if="this.disabled" class="roll-results">
      <p>{{ this.progressTrack }} {{ this.challengeDice }}</p>

      <div v-if="this.progressResult === 2">
        <p>Strong hit</p>
        <p v-html="progressMoveSelected.strong"></p>
      </div>
      <div v-if="this.progressResult === 1">
        <p>Weak hit</p>
        <p v-html="progressMoveSelected.weak"></p>
      </div>
      <div v-if="this.progressResult === 0">
        <p>Miss</p>
        <p v-html="progressMoveSelected.miss"></p>
      </div>
      <div v-if="this.progressResult[0] === this.challengeDice[1]"><strong>Match!</strong></div>
    </div>
    <button @click="rollProgress(progressTrack)">ROLL PROGRESS</button> <button @click="clearProgress()">CLEAR</button>
  </div>
</template>

<script>
import movesJson from "@/assets/moves.json";
export default {
  data() {
    return {
      movesList: movesJson,
      progressMoveSelected: "none",
      challengeDice: [0, 0],
      progressResult: 0,
      progressTrack: 0,
      disabled: false,
    };
  },
  computed: {
    movesFilter: function() {
      return this.movesList.filter((m) => {
        return m.type === "progress";
      });
    },
  },
  methods: {
    rollDice(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    },
    rollProgress(progress) {
      this.progressResult = 0;
      this.challengeDice = [this.rollDice(1, 10), this.rollDice(1, 10)];
      this.challengeDice.forEach((result) => {
        if (parseInt(progress) > result) this.progressResult += 1;
      });
      this.disabled = true;
    },
    clearProgress() {
      this.progressResult = 0;
      this.disabled = false;
    },
  },
};
</script>

<style></style>
