<template>
  <div v-on:click="rolla()" :class="type" class="die">
    <div class="die--value">{{ diceroll }}</div>
  </div>
</template>

<script>
import roll from "roll-dice";
import Vue from "vue";
export default {
  name: "die",
  props: {
    dieFaces: Array,
    type: String,
    bus: Vue
  },
  data: function() {
    return {
      diceroll: "❓"
    };
  },
  methods: {
    rolla: function() {
      const ro = new roll();
      const diceString = `[${this.dieFaces.join().replace(/,/g, "|")}]`;
      let result = ro.roll(diceString).result;
      this.diceroll = result;
      this.$emit("rolled", result);
    }
  },
  mounted() {
    this.bus.$on("roll", this.rolla);
  }
};
</script>

<style>
.die {
  max-width: 50px;
  width: 50px;
  height: 50px;
  border-radius: 8px;
  cursor: pointer;
  border: 2px solid #666;
}
.die--value {
  padding: 10px;
  user-select: none;
  font-size: 30px;
}
.die:active {
  background: black;
}
.attack {
  background: white;
}
.magic {
  background: lightblue;
}
.defence {
  background: black;
}
.defence:active {
  background: white;
}
.die.success {
  border-color: greenyellow;
}
.die.failure {
  border-color: red;
}
/* .hammer{ background: lightblue}; */
</style>
