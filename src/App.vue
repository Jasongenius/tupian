<template>
  <div id="app">
    <TypingGame />
  </div>
</template>

<script>
import TypingGame from './components/TypingGame.vue';

export default {
  name: 'App',
  components: {
    TypingGame,
  },
};
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f0f0;
}
</style>