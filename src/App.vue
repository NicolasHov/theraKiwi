<template>
  <div id="app">
    <Board />
    <div class="fabric">
      <canvas ref="canvasRef" width="1000" height="600">
        <Card v-for="card in cards" :key="card.id" :value="card.value" :imageUrl="card.imageUrl"
          :is-flipped="card.isFlipped" @flip="flip(card)" />
      </canvas>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { fabric } from 'fabric';
import Board from './components/Board.vue';
import Card from './components/Card.vue';

let canvas = new fabric.Canvas('c', {
  backgroundColor: '#333',
  HOVER_CURSOR: 'pointer'
});

interface CardItem {
  id: number;
  value: string;
  isFlipped: boolean;
  imageUrl: string
}

export default defineComponent({
  components: {
    Board,
    Card
  },
  mounted() {
    const cards = ref<CardItem[]>([
      { id: 1, value: 'A', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQt3YWxmE3an3J-mG-8L4Uo9oxjkQQbUOVa90DgZYwmna3ryuIDbtwek-7Q4gjqOKo21N8&usqp=CAU" },
      { id: 2, value: 'B', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSmv-j9XdV9hj6EZQ1W2SUDtqO46SlVW3oIvtxvZ0CZk1D1b0COqafOIyVsqdWuVgn0zNA&usqp=CAU" },
      { id: 3, value: 'C', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTHjl3uUmP-SeLbFFLtxrSqK8UGsL2UUfVbqVFvHbNySJv4Lnh3t8rNjZnK2khjpRj2N78&usqp=CAU" },
      { id: 4, value: 'D', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRQhxO9_irjM8QqKIfWa91qMHWm0M5_v2iQSvJGV0s6nxmtCgoNfLzwgEwKWF5_7ViuT8I&usqp=CAU" },
      // Add more cards as needed
    ])

    canvas = new fabric.Canvas(this.$refs.canvasRef, {
      isDrawingMode: false
    })

    const rect = new fabric.Rect({
      top: 100,
      left: 100,
      width: 50,
      height: 50,
      fill: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTHjl3uUmP-SeLbFFLtxrSqK8UGsL2UUfVbqVFvHbNySJv4Lnh3t8rNjZnK2khjpRj2N78&usqp=CAU"
    })

    // const truc = new fabric.Image.fromURL('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTHjl3uUmP-SeLbFFLtxrSqK8UGsL2UUfVbqVFvHbNySJv4Lnh3t8rNjZnK2khjpRj2N78&usqp=CAU', function (img: any) {
    //   img.scale(0.5).set({
    //     left: 100,
    //     top: 100,
    //     angle: -15,
    //     clipPath: new fabric.Circle({
    //       //radius: radius,
    //       originX: 'center',
    //       originY: 'center',
    //     }),
    //   });
    // })
    // canvas.add(truc)
    canvas.add(rect)

    return {
      cards
    }
  }
});
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.fabric {
  height: 500px;
  display: flex;
  align-items: center;
  justify-content: center;
}

canvas {
  border: 1px solid black;
  border-radius: 8px;
  ;
}
</style>
