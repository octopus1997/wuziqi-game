<template>
  <div class="app">
    <header>
      <h1>五子棋游戏</h1>
    </header>
    <main>
      <WuziqiBoard />
    </main>
  </div>
</template>

<script setup>
import WuziqiBoard from './components/WuziqiBoard.vue'
</script>

<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

header {
  text-align: center;
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

header h1 {
  margin: 0;
  font-size: 2rem;
}

main {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}
</style>
