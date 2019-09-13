<template>
  <div class="dice-box">
    <header>
      <h2>{{ type.toUpperCase() }} DICE</h2>
      <ul class="dice-controls">
        <li v-on:click="addDie()">Add</li>
        <li v-on:click="removeDie()">Remove</li>
        <li v-on:click="resetDice()">Clear</li>
        <li v-on:click="rollAll()">Roll All</li>
      </ul>
    </header>
    <div class="dice-area">
      <die
        v-for="(die, index) in dice"
        v-bind:key="index"
        v-bind:type="die.type"
        :bus="bus"
        :dieFaces="dieFaces"
        @rolled="dieResult(index, ...arguments)"
      />
    </div>
    <div class="check-area">
      <div class="success-checkbox" v-for="face in uniqueFaces" v-bind:key="face">
        <label for="face">{{face}}</label>
        <input type="checkbox" :value="face" v-model="checkedResults" />
        <span>{{getPercent(face)}}%</span>
      </div>
    </div>
    <div class="result-box">
      <div>Chance of success = {{chanceOfSuccess()}}%</div>
      <div>{{isSuccess() ? 'Success!' : 'Failure'}}</div>
    </div>
  </div>
</template>

<script>
import Die from "./Die";
import Vue from "vue";

export default {
  name: "dice-box",
  components: {
    Die
  },
  props: {
    dieFaces: Array,
    type: String
  },
  data: function() {
    return {
      dice: [],
      bus: new Vue(),
      uniqueFaces: new Set(this.dieFaces),
      checkedResults: [],
      faceProbabilities: this.calculateFaceProbabilities(),
      what: "cake"
    };
  },
  methods: {
    addDie: function() {
      this.dice.push({ type: this.type, value: "?" });
    },
    removeDie: function() {
      this.dice.pop();
    },
    resetDice: function() {
      this.dice = [];
    },
    rollAll: function() {
      this.bus.$emit("roll");
    },
    dieResult: function(index, value) {
      this.dice[index].value = value;
      this.success = this.isSuccess();
    },
    calculateFaceProbabilities: function() {
      var counts = {};
      var probs = {};
      var total = this.dieFaces.length;
      this.dieFaces.forEach(x => {
        counts[x] = (counts[x] || 0) + 1;
      });
      for (var key in counts) {
        probs[key] = counts[key] / total;
      }
      return probs;
    },
    getPercent: function(face) {
      return (parseFloat(this.faceProbabilities[face]) * 100).toFixed(0);
    },
    isSuccess: function() {
      var success = false;
      if (this.dice.length < 1) {
        return false;
      }
      this.checkedResults.forEach(res => {
        this.dice.forEach(die => {
          if (res === die.value) {
            success = true;
          }
        });
      });
      return success;
    },
    chanceOfSuccess: function() {
      // 1 - ((length - x) ^ n / l^n)
      var counts = {};
      var faces = this.dieFaces.length;
      counts = this.dieFaces.forEach(x => {
        counts[x] = (counts[x] || 0) + 1;
      });
      var x = 0;
      this.checkedResults.forEach(res => {
        x += counts[res];
      });
      var prob =
        1 -
        Math.pow(faces - x, this.dice.length) /
          Math.pow(faces, this.dice.length);
      return (prob * 100).toFixed(2);
    }
  }
};
</script>
<style scoped>
.dice-box {
  display: grid;
  grid-template-columns: 80% 20%;
  grid-template-rows: 150px 1fr 25px;
  /* min-height: 500px; */
}
.dice-controls {
  list-style-type: none;
  margin: 0;
  padding: 0;
}
li {
  display: inline;
  border: 1px solid;
  border-radius: 10px;
  padding: 5px 10px;
  margin-right: 10px;
}
header {
  grid-row-start: 1;
}
.dice-area {
  grid-column-start: 1;
  grid-row-start: 2;
  display: flex;
  flex-wrap: wrap;
  min-height: 100px;
  /* grid-template-columns: repeat(10, 10%); */
}
.check-area {
  grid-column-start: 2;
  grid-row-start: 2;
}
.success-checkbox {
  border: 1px dotted blue;
}
.result-box {
  grid-row-start: -1;
}
</style>
