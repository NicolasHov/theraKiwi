<template>
  <div id="app">
    <Board />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Board from './components/Board.vue';

export default defineComponent({
  components: {
    Board,
  },
});
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
