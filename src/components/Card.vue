<template>
    <div class="card" :class="{ flipped: isFlipped }" @click="flip">
        <div class="card-inner">
            <div class="card-back">{{ value }}</div>
            <div class="card-front">
                <img :src="imageUrl" alt="card cover" />
            </div>
            <div class="card-back"></div>
        </div>
    </div>
</template>
  
<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
    props: {
        value: String,
        isFlipped: Boolean,
        imageUrl: String
    },
    methods: {
        flip() {
            // if (!this.isFlipped) {
            this.$emit('flip', this.id);
            // }
        },
    },
});
</script>
  
<style scoped>
.card {
    /* perspective: 1000px; ???*/

    cursor: pointer;
    width: 200px;
    height: 260px;
    border: 1px solid #CCC;
    margin: 40px 0;
    perspective: 600px;
}


.card-inner {
    line-height: 260px;
    color: white;
    text-align: center;
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
    transform: rotateY(180deg);
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
  