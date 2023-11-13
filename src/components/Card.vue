<template>
    <div class="card" :class="{ flipped: isFlipped, matched: isMatched }" @click="flipCard">
        <div class="card-inner">
            <div class="card-front"></div>
            <div class="card-back">{{ value }}</div>
        </div>
    </div>
</template>
  
<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
    props: {
        value: String,
        isFlipped: Boolean,
        isMatched: Boolean,
    },
    methods: {
        flipCard() {
            if (!this.isFlipped) {
                console.log(this.value);
                this.$emit('flip', this.value);
            }
        },
    },
});
</script>
  
<style scoped>
.card {
    width: 100px;
    height: 150px;
    perspective: 1000px;
    cursor: pointer;
}

.card-inner {
    width: 100%;
    height: 100%;
    transform-style: preserve-3d;
    transition: transform 0.6s;
}

.card.flipped .card-inner {
    transform: rotateY(180deg);
}

.card-front,
.card-back {
    width: 100%;
    height: 100%;
    position: absolute;
    backface-visibility: hidden;
}

.card-front {
    background-color: #000000;
}

.card-back {
    background-color: #4caf50;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
}
</style>
  