<script setup>
import { ref } from 'vue';
const props = defineProps({ 'customs': Object, 'selectedColor': Number })
</script>

<template>
    <div class="pegs">
        <template v-for="i in customs.numColors">
            <input type="button" :class="{ active: i - 1 === props.selectedColor, peg: true }"
                @click="$emit('setSelectedColor', i - 1)"
                :style="{ background: `radial-gradient(circle at 30% 30%, #fff, ${customs.colors[i - 1]} 15%, #000)` }" />
        </template>
        <img :class="{ active: -1 === props.selectedColor, peg: true }" src="../assets/cancel.svg" alt="cancel"
            @click="$emit('setSelectedColor', -1)" />
        <img class="peg" src="../assets/check.svg" alt="check" @click="$emit('checkLine')" />
    </div>
</template>

<style scoped>
.pegs {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100vw;
    display: flex;
    justify-content: center;
}

.peg {
    aspect-ratio: 1;
    width: 5%;
    margin: 2%;
    border-radius: 50%;
    border: none;
    box-shadow: 6px 6px 4px 0 #00000080;
    transition: all 0.5s;
    transform-origin: center;
}

.peg:hover,
.active.peg {
    box-shadow: 6px 6px 4px 0 #ffffff80;
    transform: scale(1.2);
}
</style>