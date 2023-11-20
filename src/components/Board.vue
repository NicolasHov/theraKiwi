<template>
    <div class="board">
        <Card v-for="card in cards" :key="card.id" :value="card.value" :imageUrl="card.imageUrl"
            :is-flipped="card.isFlipped" @flip="flip(card)" />
    </div>
    <button @click="unflipCards()">Unflip cards</button>
    <div>
        <!-- from https://konvajs.org/docs/select_and_transform/Resize_Limits.html -->
        <v-stage ref="stage" :config="configKonva" @mousedown="handleStageMouseDown" @touchstart="handleStageMouseDown">
            <v-layer>
                <v-text :config="{ text: 'TheraKiwi', fontSize: 15 }" />
                <v-rect v-for="card in cards" :key="card.id" :value="card.value" :imageUrl="card.imageUrl"
                    :is-flipped="card.isFlipped" @flip="flip(card)" @transformend="handleTransformEnd" :config="{
                        x: 20 + (card.id - 1) * 120,
                        y: 50,
                        width: 100,
                        height: 100,
                        fill: '#333' + (card.id + 1) * 10,
                        name: 'rect',
                        stroke: 'black',
                        draggable: true,
                    }" />
                <v-transformer ref="transformer" />
                <!-- <v-layer ref="dragLayer" -->
                ></v-layer>
        </v-stage>
    </div>
</template>
  
<script lang="ts">
import { defineComponent, ref } from 'vue';
import Card from './Card.vue';

const width = window.innerWidth;
const height = window.innerHeight;

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
        const configKonva = {
            width: width,
            height: height
        }

        const configCircle = {
            x: 100,
            y: 100,
            radius: 70,
            fill: "red",
            stroke: "black",
            strokeWidth: 4
        }

        const cards = ref<CardItem[]>([
            { id: 1, value: 'A', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQt3YWxmE3an3J-mG-8L4Uo9oxjkQQbUOVa90DgZYwmna3ryuIDbtwek-7Q4gjqOKo21N8&usqp=CAU" },
            { id: 2, value: 'B', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSmv-j9XdV9hj6EZQ1W2SUDtqO46SlVW3oIvtxvZ0CZk1D1b0COqafOIyVsqdWuVgn0zNA&usqp=CAU" },
            { id: 3, value: 'C', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTHjl3uUmP-SeLbFFLtxrSqK8UGsL2UUfVbqVFvHbNySJv4Lnh3t8rNjZnK2khjpRj2N78&usqp=CAU" },
            { id: 4, value: 'D', isFlipped: false, imageUrl: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRQhxO9_irjM8QqKIfWa91qMHWm0M5_v2iQSvJGV0s6nxmtCgoNfLzwgEwKWF5_7ViuT8I&usqp=CAU" },
            // Add more cards as needed
        ])
        let flippedCards = ref<CardItem[]>([])

        const flip = (clickedCard: CardItem) => {
            // Flip the clicked card
            const foundCard = cards.value.find((card) => card.id === clickedCard.id);

            if (foundCard) {
                foundCard.isFlipped = !foundCard.isFlipped;
                if (foundCard.isFlipped) flippedCards.value.push(foundCard)
                else flippedCards.value = flippedCards.value.filter(card => card.id !== foundCard.id)
            }
        }

        const unflipCards = () => {
            flippedCards.value.forEach((card) => {
                flip(card)
            });
            flippedCards.value = [];
        };

        const handleTransformEnd = (e) => {
            // shape is transformed, let us save new attrs back to the node
            // find element in our state
            // const rect = this.rectangles.find(
            //     (r) => r.name === this.selectedShapeName
            // );
            const foundCard = cards.value.find((card) => card.id === clickedCard.id);
            // add test
            // update the state
            foundCard.x = e.target.x();
            foundCard.y = e.target.y();
            foundCard.rotation = e.target.rotation();
            foundCard.scaleX = e.target.scaleX();
            foundCard.scaleY = e.target.scaleY();

            // change fill
            // foundCard.fill = Konva.Util.getRandomColor();
        }

        const handleStageMouseDown = (e) => {
            // clicked on stage - clear selection
            if (e.target === e.target.getStage()) {
                this.selectedShapeName = '';
                this.updateTransformer();
                return;
            }

            // clicked on transformer - do nothing
            const clickedOnTransformer =
                e.target.getParent().className === 'Transformer';
            if (clickedOnTransformer) {
                return;
            }

            // find clicked rect by its name
            const name = e.target.name();
            const rect = this.rectangles.find((r) => r.name === name);
            if (rect) {
                this.selectedShapeName = name;
            } else {
                this.selectedShapeName = '';
            }
            this.updateTransformer();
        }

        const updateTransformer = () => {
            // here we need to manually attach or detach Transformer node
            const transformerNode = this.$refs.transformer.getNode();
            const stage = transformerNode.getStage();
            const { selectedShapeName } = this;

            const selectedNode = stage.findOne('.' + selectedShapeName);
            // do nothing if selected node is already attached
            if (selectedNode === transformerNode.node()) {
                return;
            }

            if (selectedNode) {
                // attach to another node
                transformerNode.nodes([selectedNode]);
            } else {
                // remove transformer
                transformerNode.nodes([]);
            }
        }

        return {
            cards,
            flippedCards,
            configKonva,
            configCircle,
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
  