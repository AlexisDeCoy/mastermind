<script setup>
const props = defineProps({ customs: Object })
</script>

<template>
    <aside>
        <h2 class="label">Include Blanks:</h2>
        <section>
            <h2 :class="props.customs.includeBlanks ? 'active' : ''" @click="$emit('setIncludeBlanks', true)">Yes</h2>
            <h2 :class="props.customs.includeBlanks ? '' : 'active'" @click="$emit('setIncludeBlanks')">No</h2>
        </section>
        <h2 class="label">Colors:</h2>
        <section>
            <img src="../assets/decrease.svg" alt="decrease" @click="$emit('setNumColors')">
            <div class="value">{{ customs.numColors }}</div>
            <img src="../assets/increase.svg" alt="increase" @click="$emit('setNumColors', true)">
        </section>
        <h2 class="label">Lines:</h2>
        <section>
            <img src="../assets/decrease.svg" alt="decrease" @click="$emit('setNumLines')">
            <div class="value">{{ customs.numLines }}</div>
            <img src="../assets/increase.svg" alt="increase" @click="$emit('setNumLines', true)">
        </section>
        <h2 class="label">Pegs:</h2>
        <section>
            <img src="../assets/decrease.svg" alt="decrease" @click="$emit('setNumPegs')">
            <div class="value">{{ customs.numPegs }}</div>
            <img src="../assets/increase.svg" alt="increase" @click="$emit('setNumPegs', true)">
        </section>
        <h2 class="label">Colors:</h2>
        <div class="colors">
            <div class="color" v-for="(input, index) in props.customs.numColors">
                <input type="color" :value="props.customs.colors[index]"
                    @change="e => { $emit('setColor', index, e.target.value) }">
            </div>
        </div>
    </aside>
</template>

<style scoped>
aside {
    display: flex;
    flex-direction: column;
    height: 100%;
    color: #fff;
    text-align: center;
    width: 30%;
    position: relative;
    overflow-y: auto;
    scrollbar-gutter: stable;
    scrollbar-color: #fff transparent;
}

h1 {
    font-size: calc(2vw + 10px);
    line-height: 1;
    margin: 0 1vw;
    width: 100%;
    position: absolute;
    bottom: 100%;
}

section {
    height: 10%;
    display: flex;
    max-width: 100%;
    align-items: center;
    justify-content: center;
}

.label {
    height: 7%;
}

.input {
    height: 60%;
    display: flex;
    align-items: center;
    max-width: 100%;
    justify-content: center;
}

section>h2 {
    padding: 0 10%;
    box-sizing: border-box;
    cursor: pointer;
}

.active {
    text-decoration: underline;
}

img {
    max-height: 80%;
}

.value {
    font-size: calc(4vw + 5px);
    line-height: 1;
    margin: 0 1vw;
    width: 20%;
}

.colors {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
    width: 100%;
}

.color {
    width: 29%;
    aspect-ratio: 1;
    margin: 2%;
}

input {
    height: 100%;
    width: 100%;
}
</style>