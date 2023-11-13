<template>
    <div class="board">
        <Card v-for="card in cards" :key="card.id" :value="card.value" :is-flipped="card.isFlipped"
            :is-matched="card.isMatched" @flip="handleCardFlip(card)" />
    </div>
</template>
  
<script lang="ts">
import { defineComponent, ref } from 'vue';
import Card from './Card.vue';

interface CardItem {
    id: number;
    value: string;
    isFlipped: boolean;
    isMatched: boolean;
}

export default defineComponent({
    components: {
        Card,
    },
    data() {
        return {
            cards: [
                { id: 1, value: 'A', isFlipped: false, isMatched: false },
                { id: 2, value: 'B', isFlipped: false, isMatched: false },
                { id: 3, value: 'C', isFlipped: false, isMatched: false },
                { id: 4, value: 'D', isFlipped: false, isMatched: false },
                // Add more cards as needed
            ] as CardItem[],
            flippedCards: [] as CardItem[],
        };
    },
    methods: {
        handleCardFlip(clickedCard: CardItem) {
            // if (this.flippedCards.length > 0) {


            // } else {
            console.log("clickedCard: ", clickedCard);
            // Flip the clicked card
            clickedCard.isFlipped = true;
            this.flippedCards.push(clickedCard);
            // }

        },
        //   checkMatch() {
        //     const [firstCard, secondCard] = this.flippedCards;

        //     if (firstCard.value === secondCard.value) {
        //       this.markAsMatched();
        //     } else {
        //       this.unflipCards();
        //     }
        //   },
        //   markAsMatched() {
        //     this.flippedCards.forEach((card) => {
        //       card.isMatched = true;
        //     });

        //     this.flippedCards = [];
        //   },
        unflipCards() {
            setTimeout(() => {
                this.flippedCards.forEach((card) => {
                    card.isFlipped = false;
                });

                this.flippedCards = [];
            }, 1000);
        },
    },
});
</script>
  
<style scoped>
.board {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
}
</style>
  