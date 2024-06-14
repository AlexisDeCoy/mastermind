<script setup>
import { computed, ref, watch } from 'vue';
import Board from './components/Board.vue'
import Customize from './components/Customize.vue'
import Pop from './components/Pop.vue';

//Customizations
const customs = ref({
  colors: ['#ff0000', '#0000ff', '#008000', '#ffff00', '#111111', '#eeeeee', '#800080', '#ffc0cb', '#00ffae'],
  includeBlanks: false,
  numColors: 6,
  numLines: 10,
  numPegs: 4
})

//Game Vars
const won = ref(false)
const activeLine = ref(1)
const actualColors = ref(setActual())
const boardKey = ref(0)

watch(actualColors, () => console.log(actualColors.value))

function setActual() {
  let actual = []
  for (let i = 0; i < customs.value.numPegs; i++) {
    if (customs.valueincludeBlanks) {
      actual.push(Math.floor(Math.random() * (customs.value.numColors + 1)) - 1)
    }
    else actual.push(Math.floor(Math.random() * customs.value.numColors))
  }
  return actual
}

function setIncludeBlanks(include) {
  if (include) customs.value.includeBlanks = true
  else customs.value.includeBlanks = false
  newGame()
}

function setNumColors(increase) {
  if (increase && customs.value.numColors < 9) customs.value.numColors = customs.value.numColors + 1
  else if (!increase && customs.value.numColors > 2) customs.value.numColors = customs.value.numColors - 1
  newGame()
}

function setNumLines(increase) {
  if (increase && customs.value.numLines < 15) customs.value.numLines = customs.value.numLines + 1
  else if (!increase && customs.value.numLines > 4) customs.value.numLines = customs.value.numLines - 1
  newGame()
}

function setNumPegs(increase) {
  if (increase && customs.value.numPegs < 8) customs.value.numPegs = customs.value.numPegs + 1
  else if (!increase && customs.value.numPegs > 2) customs.value.numPegs = customs.value.numPegs - 1
  newGame()
}

function setColor(index, value) {
  let newColors = [...customs.value.colors]
  newColors[index] = value
  customs.value.colors = newColors
}

function incrementLine() {
  activeLine.value = activeLine.value + 1
}

function setWon() {
  won.value = true
}

function newGame() {
  console.log('set')
  actualColors.value = setActual()
  activeLine.value = 1
  won.value = false
}

</script>

<template>
  <main>
    <Board :key="boardKey" :customs="customs" :activeLine="activeLine"
      :actualColors="actualColors" @incrementLine="incrementLine" @setWon="setWon" />
    <Customize v-if="activeLine <= customs.numLines && !won" :customs="customs" @setIncludeBlanks="setIncludeBlanks" @setNumColors="setNumColors"
      @setNumLines="setNumLines" @setNumPegs="setNumPegs" @setColor="setColor" />
    <Pop v-else :customs="customs" :actualColors="actualColors" :activeLine="activeLine" :won="won" @newGame="newGame" />
  </main>
</template>

<style scoped>
main {
  height: 60vh;
  display: flex;
  margin: 0 5%;
  align-items: center;
  justify-content: space-between;
}
</style>
