<template>
  <div>
    <h1>ACTION MOVES</h1>

    <div class="roll-options">
      <p>Select your move and roll for result (if that is the case).</p>
      1 type of move: <select v-model="moveType" name="moves-types" id="moves-types" :disabled="disabled">
        <option value="none" selected> </option>
        <option value="adventure">Adventure</option>
        <option value="relationship">Relationship</option>
        <option value="combat">Combat</option>
        <option value="suffer">Suffer</option>
        <option value="quest">Quest</option>
      </select>

      <div v-if="this.moveType != 'none'">
       2 action move: <select v-model="moveSelection" name="moves" id="moves" :disabled="disabled">
         <option value="none" selected> </option>
         <option v-for="move in movesFilter" :key="move.desc" :value="move">{{move.title}}</option>
         </select>
      </div>

      <div v-if="this.moveSelection != 'none'" class="move-card">
        <h3>{{this.moveSelection.title}}</h3>

        <h4>Description</h4>

        <p v-html="moveSelection.desc"></p>

      </div>

        <p v-if="this.showStatHelp">Roll with the indicated stat <strong>in the description</strong> otherwise:
          <ul>
            <li>• With speed, agility, or precision: Roll +edge.</li>
            <li>• With charm, loyalty, or courage: Roll +heart.</li>
            <li>• With aggressive action, forceful defense, strength, or endurance: Roll +iron.</li>
            <li>• With deception, stealth, or trickery: Roll +shadow.</li>
            <li>• With expertise, insight, or observation: Roll +wits.</li>
          </ul>
        </p>
      <p><strong>Stat:</strong> <button @click="showStatHelp = !showStatHelp">?</button></p>
      <input v-model="rollStat" type="number" name="stat" id="stat" :disabled="disabled" />

      <p><strong>Adds:</strong></p>
      <input v-model="rollAdd" type="number" name="add" id="add" :disabled="disabled" />
    </div>

    <div v-if="this.actionDice > 0" class="roll-results">
      <p>{{ this.actionDice }} ({{ this.rollStat }} + {{ this.rollAdd }}) {{ this.challengeDice }}</p>

      <div v-if="this.actionResult === 2">
        <p>Strong hit</p>
        <p v-html="moveSelection.strong"> </p>  
      </div>
      <div v-if="this.actionResult === 1">
        <p>Weak hit</p>
        <p v-html="moveSelection.weak"> </p>    
      </div>
      <div v-if="this.actionResult === 0">
        <p>Miss</p>
        <p v-html="moveSelection.miss"> </p>    
      </div>
      <div v-if="this.challengeDice[0] === this.challengeDice[1]"><strong>Match!</strong></div>

      <div v-if="this.actionResult === 0 && (this.moveSelection=== 'Endure Harm' || this.moveSelection=== 'Endure Stress')">
        <h3>Roll Table: </h3>
        <table v-if="this.moveSelection=== 'Endure Harm'"><thead><tr><th>Roll</th><th>Result</th></tr></thead><tbody><tr><td>1-10</td><td>The harm is mortal. Face Death.</td></tr><tr><td>11-20</td><td>You are dying. You need to Heal within an hour or two, or<br>Face Death.</td></tr><tr><td>21-35</td><td>You are unconscious and out of action. If left alone, you<br>come back to your senses in an hour or two. If you are<br>vulnerable to a foe not inclined to show mercy, Face Death</td></tr><tr><td>36-50</td><td>You are reeling and fighting to stay conscious. If you engage<br>in any vigorous activity (such as running or fighting) before<br>taking a breather for a few minutes, roll on this table again<br>(before resolving the other move).</td></tr><tr><td>51-00</td><td>You are battered but still standing.</td></tr></tbody></table>
        <table v-if="this.moveSelection=== 'Endure Stress'"><thead><tr><th>Roll</th><th>Result</th></tr></thead><tbody><tr><td>1-10</td><td>You are overwhelmed. Face Desolation.</td></tr><tr><td>11-25</td><td>You give up. Forsake Your Vow (if possible, one relevant to<br>your current crisis).<br></td></tr><tr><td>26-50</td><td>You give in to a fear or compulsion, and act against your<br>better instincts.<br></td></tr><tr><td>50-00</td><td>You persevere.<br></td></tr></tbody></table>
        <p><strong>Result:</strong> {{this.harmResult}}</p>
        <button @click="this.harmResult = rollDice(1,100)">Roll Harm</button>
      </div>
    </div>

    <button @click="rollAction()">ROLL ACTION</button> <button @click="clearAction()">CLEAR</button>
  </div>
</template>

<script>
import movesJson from '@/assets/moves.json'
export default {
  data() {
    return {
      movesList: movesJson,
      actionDice: 0,
      challengeDice: [0, 0],
      actionResult: 0,
      rollAdd: 0,
      rollStat: 0,
      moveType: "none",
      moveSelection: "none",
      disabled: false,
      harmResult: 0,
      showStatHelp: false,
    };
  },
  computed: {
    movesFilter: function() {
      return this.movesList.filter((m) => { return m.type === this.moveType})
    }
  },
  methods: {
    rollDice(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    },
    rollAction() {
      this.actionResult = 0;
      this.actionDice = this.rollDice(1, 6) + parseInt(this.rollAdd) + parseInt(this.rollStat);
      this.challengeDice = [this.rollDice(1, 10), this.rollDice(1, 10)];
      this.challengeDice.forEach((result) => {
        if (this.actionDice > result) this.actionResult += 1;
      });
      this.disabled = true;
    },
    clearAction() {
      this.actionResult = 0;
      this.actionDice = 0;
      this.disabled = false;
    },
  },
};
</script>

<style>
ul li {list-style: none;}
</style>
