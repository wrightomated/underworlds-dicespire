<template>
  <div id="app">
    <dice-box class="dice-container" type="attack" :dieFaces="attackDieFaces" />
    <dice-box class="dice-container" type="magic" :dieFaces="magicDieFaces" />
    <dice-box class="dice-container" type="defence" :dieFaces="defenceDieFaces" />
  </div>
</template>

<script>
import DiceBox from "./components/DiceBox";

export default {
  name: "app",
  components: {
    DiceBox
  },
  data: function() {
    return {
      attackDieFaces: ["💥", "🔨", "🔨", "⚔️", "🌓", "🌕"],
      defenceDieFaces: ["💥", "🛡", "🛡", "♻", "🌓", "🌕"],
      magicDieFaces: ["💥", "🌀", "🌀", "⚡", "⚡", "⚡"]
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  /* -webkit-font-smoothing: antialiased; */
  /* -moz-osx-font-smoothing: grayscale; */
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
}
.dice-container {
  flex: 1;
}
</style>
