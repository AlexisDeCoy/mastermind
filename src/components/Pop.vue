<script setup>
import { computed } from 'vue';
const props = defineProps({ 'customs': Object, 'won': Boolean, 'actualColors': Array, 'activeLine': Number, 'orientation': String })

const lineStyle = computed(() => ({
    aspectRatio: `1 / ${Math.ceil(props.customs.numPegs / 2) * 0.5 + props.customs.numPegs}`,
    maxWidth: `40%`
}))

const pinsStyle = computed(() => ({
    aspectRatio: `1 / ${Math.ceil(props.customs.numPegs / 2) * 0.5}`,
    width: '96%'
}))
</script>

<template>
    <aside :class="{portrait: orientation === 'portrait'}" @click="$emit('newGame')">
        <div class="line" :style="lineStyle">
            <div class="pins" :style="pinsStyle"></div>
            <div class="peg" v-for="color in actualColors"
                :style="{ background: `radial-gradient(circle at 30% 30%, #fff, ${customs.colors[color]} 15%, #000)` }">
            </div>
        </div>
        <div class="text" v-if="props.won">
            <h1 style="font-size: 3em;">You Won!</h1>
            <h2>Lines Used:</h2>
            <h1 style="font-size: 6em;">{{ props.activeLine }}</h1>
        </div>
        <div class="text" v-else>
            <h1 style="font-size: 3em;">Try Again</h1>
        </div>
    </aside>
</template>

<style scoped>
aside {
    display: flex;
    height: 100%;
    color: #fff;
    text-align: center;
    width: 30%;
    padding: 2% 0;
    justify-content: space-between;
    align-items: center;
}

.portrait {
    font-size: .6em;
    flex-direction: column;
}

.line {
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
}

.text {
    width: 60%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    color: #fff;
}

.peg {
    aspect-ratio: 1;
    border-radius: 50%;
    box-shadow: 6px 6px 4px 0 #00000080;
    width: 94%;
}
</style>