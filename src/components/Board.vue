<template>
    <div class="board">
        <Card v-for="card in cards" :key="card.id" :value="card.value" :imageUrl="card.imageUrl"
            :is-flipped="card.isFlipped" @flip="flip(card)" />
    </div>
    <button @click="unflipCards()">Unflip cards</button>
</template>
  
<script lang="ts">
import { defineComponent, ref } from 'vue';
import Card from './Card.vue';

interface CardItem {
    id: number;
    value: string;
    isFlipped: boolean;
    imageUrl: string
}

export default defineComponent({
    components: {
        Card,
    },
    setup() {
        const cards = ref<CardItem[]>([
            { id: 1, value: 'A', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQt3YWxmE3an3J-mG-8L4Uo9oxjkQQbUOVa90DgZYwmna3ryuIDbtwek-7Q4gjqOKo21N8&usqp=CAU" },
            { id: 2, value: 'B', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSmv-j9XdV9hj6EZQ1W2SUDtqO46SlVW3oIvtxvZ0CZk1D1b0COqafOIyVsqdWuVgn0zNA&usqp=CAU" },
            { id: 3, value: 'C', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTHjl3uUmP-SeLbFFLtxrSqK8UGsL2UUfVbqVFvHbNySJv4Lnh3t8rNjZnK2khjpRj2N78&usqp=CAU" },
            { id: 4, value: 'D', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRQhxO9_irjM8QqKIfWa91qMHWm0M5_v2iQSvJGV0s6nxmtCgoNfLzwgEwKWF5_7ViuT8I&usqp=CAU" },
            // Add more cards as needed
        ])
        const flippedCards = ref<CardItem[]>([])

        const flip = (clickedCard: CardItem) => {
            // Flip the clicked card
            const foundCard = cards.value.find((card) => card.id === clickedCard.id);

            if (foundCard) {
                foundCard.isFlipped = !foundCard.isFlipped;
                if (foundCard.isFlipped) flippedCards.value.push(foundCard)
            }
        }

        const unflipCards = () => {
            flippedCards.value.forEach((card) => {
                flip(card)
            });
            flippedCards.value = [];
        };

        return {
            cards,
            flippedCards,
            flip,
            unflipCards
        }
    }
});
</script>
  
<style scoped>
.board {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
}
</style>
  