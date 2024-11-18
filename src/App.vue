<script setup>
import { ref } from 'vue'

let turn = 0
const board = ref(Array(9).fill(null))
const score = ref('')
const gameOver = ref(false)

const btnPressed = (event, index) => {
  if (board.value[index] || gameOver.value) return
  turn++
  const currentPlayer = turn % 2 ? 'orange' : 'green'
  board.value[index] = currentPlayer

  const btn = event.target
  btn.style.backgroundColor = turn % 2 ? 'salmon' : 'lightgreen'
  // btn.textContent =  currentPlayer;
  // alert(winner)
  if (winner()) {
    score.value=`Player ${currentPlayer} wins!`;
    gameOver.value = true;
    setTimeout(resetBoard, 1500)
  } else if (turn === 9) {
    score.value='Tie!';
    gameOver.value = true;
    setTimeout(resetBoard, 1500)
  }
}

const winner = () => {
  const b = board.value
  const winPatterns = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ]
  return winPatterns.some(
    (pattern) =>
      b[pattern[0]] && b[pattern[0]] === b[pattern[1]] && b[pattern[1]] === b[pattern[2]],
  )
}

const resetBoard = () => {
  board.value = Array(9).fill(null)
  turn = 0
  gameOver.value = false
  const btns = document.querySelectorAll('button')
  btns.forEach((btn) => (btn.style.backgroundColor = ''))
}
</script>

<template>
  <main>
    <div>TIC-TAC-TOE</div>
    <div class="score">{{ score }}</div>
    <div class="container">
      <button v-for="(cell, index) in board" :key="index" @click="(event) => btnPressed(event, index)" :disabled="gameOver"></button>
    </div>
  </main>
</template>

<style scoped>
main {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

button {
  width: 100px;
  height: 100px;
}

.score {
  margin-bottom: 20px;
  font-size: 24px;
  font-weight: bold;
  color: antiquewhite;
}

.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  width: 300px;
  align-self: center;

}
@media (min-width: 720px) {
  .container{
    width: 600px;
  }
  button {
    width: 200px;
    height: 200px;
  }
  }

</style>
