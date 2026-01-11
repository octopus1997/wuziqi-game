<template>
  <div class="game-container">
    <div class="game-info">
      <div class="status">
        <span v-if="!gameOver">当前玩家: </span>
        <span class="current-player" :class="{ black: currentPlayer === 1, white: currentPlayer === 2 }">
          {{ currentPlayer === 1 ? '黑棋' : '白棋' }}
        </span>
        <span v-if="gameOver" class="winner">
          {{ winner === 1 ? '黑棋' : '白棋' }} 获胜!
        </span>
      </div>
      <button @click="resetGame" class="btn-reset">重新开始</button>
    </div>

    <div class="board-wrapper">
      <div class="board" ref="boardRef">
        <div
          v-for="(row, y) in board"
          :key="y"
          class="row"
        >
          <div
            v-for="(cell, x) in row"
            :key="`${x}-${y}`"
            class="cell"
            @click="placePiece(x, y)"
          >
            <div
              v-if="cell !== 0"
              class="piece"
              :class="{ black: cell === 1, white: cell === 2 }"
            ></div>
            <div
              v-if="lastMove.x === x && lastMove.y === y"
              class="last-move"
            ></div>
          </div>
        </div>
      </div>
    </div>

    <div v-if="gameOver" class="game-over-overlay" @click="resetGame">
      <div class="game-over-content">
        <h2>{{ winner === 1 ? '黑棋' : '白棋' }} 获胜!</h2>
        <p>点击任意处重新开始</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

// 游戏配置
const BOARD_SIZE = 15

// 游戏状态
const board = ref([])
const currentPlayer = ref(1)
const gameOver = ref(false)
const winner = ref(0)
const lastMove = reactive({ x: -1, y: -1 })

// 初始化棋盘
const initBoard = () => {
  board.value = Array(BOARD_SIZE).fill(null).map(() => Array(BOARD_SIZE).fill(0))
}

// 重置游戏
const resetGame = () => {
  initBoard()
  currentPlayer.value = 1
  gameOver.value = false
  winner.value = 0
  lastMove.x = -1
  lastMove.y = -1
}

// 落子
const placePiece = (x, y) => {
  if (gameOver.value || board.value[y][x] !== 0) return

  board.value[y][x] = currentPlayer.value
  lastMove.x = x
  lastMove.y = y

  if (checkWin(x, y)) {
    gameOver.value = true
    winner.value = currentPlayer.value
  } else {
    currentPlayer.value = currentPlayer.value === 1 ? 2 : 1
  }
}

// 检查胜利
const checkWin = (x, y) => {
  const player = board.value[y][x]
  const directions = [
    [1, 0],   // 水平
    [0, 1],   // 垂直
    [1, 1],   // 对角线
    [1, -1]   // 反对角线
  ]

  for (const [dx, dy] of directions) {
    let count = 1

    // 正方向
    let i = 1
    while (true) {
      const nx = x + dx * i
      const ny = y + dy * i
      if (nx < 0 || nx >= BOARD_SIZE || ny < 0 || ny >= BOARD_SIZE) break
      if (board.value[ny][nx] !== player) break
      count++
      i++
    }

    // 反方向
    i = 1
    while (true) {
      const nx = x - dx * i
      const ny = y - dy * i
      if (nx < 0 || nx >= BOARD_SIZE || ny < 0 || ny >= BOARD_SIZE) break
      if (board.value[ny][nx] !== player) break
      count++
      i++
    }

    if (count >= 5) return true
  }

  return false
}

// 初始化
initBoard()
</script>

<style scoped>
.game-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.game-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  max-width: 500px;
  padding: 15px 20px;
  background: #f8f9fa;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.status {
  font-size: 1.1rem;
  font-weight: 500;
}

.current-player {
  font-weight: bold;
  padding: 4px 12px;
  border-radius: 20px;
}

.current-player.black {
  background: #2c3e50;
  color: white;
}

.current-player.white {
  background: #ecf0f1;
  color: #2c3e50;
  border: 2px solid #bdc3c7;
}

.winner {
  color: #e74c3c;
  font-weight: bold;
  font-size: 1.2rem;
}

.btn-reset {
  padding: 10px 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.95rem;
  font-weight: 500;
  transition: transform 0.2s, box-shadow 0.2s;
}

.btn-reset:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.btn-reset:active {
  transform: translateY(0);
}

.board-wrapper {
  padding: 20px;
  background: #d4a574;
  border-radius: 8px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
}

.board {
  display: grid;
  grid-template-columns: repeat(15, 32px);
  grid-template-rows: repeat(15, 32px);
  gap: 0;
  background: #d4a574;
  position: relative;
}

.board::before {
  content: '';
  position: absolute;
  top: 16px;
  left: 16px;
  right: 16px;
  bottom: 16px;
  border: 2px solid #8b6914;
  pointer-events: none;
}

.row {
  display: contents;
}

.cell {
  width: 32px;
  height: 32px;
  position: relative;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
}

.cell::before,
.cell::after {
  content: '';
  position: absolute;
  background: #8b6914;
}

.cell::before {
  width: 100%;
  height: 1px;
  top: 50%;
  left: 0;
}

.cell::after {
  width: 1px;
  height: 100%;
  left: 50%;
  top: 0;
}

.cell:hover::before,
.cell:hover::after {
  background: #a67c52;
}

.piece {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  position: absolute;
  z-index: 1;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.piece.black {
  background: radial-gradient(circle at 30% 30%, #4a4a4a, #1a1a1a);
}

.piece.white {
  background: radial-gradient(circle at 30% 30%, #ffffff, #d0d0d0);
  border: 1px solid #b0b0b0;
}

.last-move {
  position: absolute;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  z-index: 2;
}

.last-move::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 8px;
  height: 8px;
  background: #e74c3c;
  border-radius: 50%;
}

.piece.black + .last-move::after {
  background: #ff6b6b;
}

.piece.white + .last-move::after {
  background: #e74c3c;
}

.game-over-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
  cursor: pointer;
}

.game-over-content {
  background: white;
  padding: 40px 60px;
  border-radius: 16px;
  text-align: center;
  animation: popIn 0.3s ease-out;
}

@keyframes popIn {
  from {
    transform: scale(0.8);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}

.game-over-content h2 {
  margin: 0 0 10px 0;
  font-size: 2rem;
  color: #2c3e50;
}

.game-over-content p {
  margin: 0;
  color: #7f8c8d;
}
</style>
