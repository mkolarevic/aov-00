<template>
  <div class="w-full h-full flex justify-center items-center flex-col">
    <h3 class="text-3xl mb-4">{{ getText() }}</h3>
    <div v-for="row in [0, 1, 2]" :key="row" class="flex">
      <div
        v-for="col in [0, 1, 2]"
        :key="`${row}${col}`"
        class="w-48 flex text-9xl my-block justify-center items-center"
        :class="getClass(fieldState[row][col])"
        @click="handleClick(row, col)"
      >
        <p>{{ fieldState[row][col] }}</p>
      </div>
    </div>
    <button class="mt-4 p-4 text-xl" v-if="gameState" @click="reset">Play again</button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
const fieldState = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
])
const playerTurn = ref('x')
const gameState = computed(() => {
  let isDraw = true
  for (let i = 0; i <= 2; i++) {
    for (const player of ['x', 'o']) {
      if (fieldState.value[i][0] === player && fieldState.value[i][1] === player && fieldState.value[i][2] === player)
        return player
      if (fieldState.value[0][i] === player && fieldState.value[1][i] === player && fieldState.value[2][i] === player)
        return player
    }

    for (let j = 0; j <= 2; j++) {
      if (!fieldState.value[i][j].length) {
        isDraw = false
      }
    }
  }
  for (const player of ['x', 'o']) {
    if (fieldState.value[0][0] === player && fieldState.value[1][1] === player && fieldState.value[2][2] === player)
      return player
    if (fieldState.value[0][2] === player && fieldState.value[1][1] === player && fieldState.value[2][0] === player)
      return player
  }

  return isDraw ? 'draw' : ''
})
const getClass = value => {
  if (value === 'x') return 'x'
  if (value === 'o') return 'o'
  return ''
}

function handleClick(row, col) {
  if (gameState.value) return
  if (!fieldState.value[row][col]) {
    fieldState.value[row][col] = playerTurn.value
    playerTurn.value = playerTurn.value === 'x' ? 'o' : 'x'
  }
}

function getText() {
  if (gameState.value) {
    if (gameState.value === 'draw') {
      return 'Draw!'
    }
    return `${gameState.value.toUpperCase()} wins!`
  }
  return `Player turn: ${playerTurn.value}`
}

function reset() {
  for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
      fieldState.value[i][j] = ''
    }
  }
  playerTurn.value = 'x'
}
</script>

<style>
.my-block {
  width: 200px;
  height: 200px;
  background: #bbb;
  margin: 8px;
}
.my-block.x {
  color: blue;
}
.my-block.o {
  color: red;
}
</style>
