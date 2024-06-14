<script setup>
import Pegs from './Pegs.vue';
import { computed, ref, watch, watchEffect } from 'vue';
const colors = ref(['red', 'blue', 'green', 'yellow', '#111', '#eee'])
const props = defineProps({ customs: Object, 'activeLine': Number, 'actualColors': Array });
const emit = defineEmits(['incrementLine', 'setWon'])
const activeColors = ref(Array(props.customs.numPegs).fill(-1));

const selectedColor = ref(0)

const lineStyle = computed(() => ({
    aspectRatio: `1 / ${Math.ceil(props.customs.numPegs / 2) * 0.5 + props.customs.numPegs}`,
    maxWidth: `calc(100% / ${props.customs.numLines})`
}))

const pinsStyle = computed(() => ({
    aspectRatio: `1 / ${Math.ceil(props.customs.numPegs / 2) * 0.5}`
}))

const data = ref([])

function setSelectedColor(colorIndex) {
    selectedColor.value = colorIndex;
}

function setActiveColor(pegIndex) {
    let newActiveColors = [...activeColors.value]
    newActiveColors[pegIndex] = selectedColor.value
    activeColors.value = newActiveColors
}

function checkLine() {
    let active = [...activeColors.value]
    let actual = [...props.actualColors]
    let exact = 0
    let near = 0

    for (let i = 0; i < props.customs.numPegs; i++) {
        if (active[i] === actual[i]) {
            exact++;
            active[i] = -2
            actual[i] = -2
        }
    }

    for (let i = 0; i < props.customs.numPegs; i++) {
        if (active[i] !== -2) {
            for (let j = 0; j < props.customs.numPegs; j++) {
                if (actual[j] !== -2 && active[i] === actual[j]) {
                    near++;
                    active[i] = -2
                    actual[j] = -2
                }
            }
        }
    }

    if (exact === props.customs.numPegs) {
        emit('setWon')
    }

    else {
        const lineData = {
            exactPins: exact,
            nearPins: near,
            slots: [...activeColors.value]
        }

        let newData = [...data.value]
        newData.push(lineData)
        data.value = newData
        activeColors.value = Array(8).fill(-1)
        emit('incrementLine')
    }
}

watch(() => props.actualColors, () => {
    activeColors.value = Array(props.customs.numPegs).fill(-1)
    data.value = []
})
</script>

<template>

    <div class="board">
        <template v-for="i in customs.numLines">
            <div :class="{ active: i === activeLine, line: true }" :style="lineStyle">
                <div class="pins" :style="pinsStyle">
                    <template v-if="i <= data.length">
                        <div class="exact pin" v-for="j in data[i - 1].exactPins"></div>
                        <div class="near pin" v-for="j in data[i - 1].nearPins"></div>
                        <div class="blank" v-for="j in customs.numPegs - data[i - 1].exactPins - data[i - 1].nearPins">
                        </div>
                    </template>
                    <div class="blank" v-else v-for="j in customs.numPegs"></div>
                </div>
                <div class="slot-container" v-for="j in customs.numPegs">
                    <div :class="{ active: i === activeLine, slot: true }"
                        @click="i === activeLine ? setActiveColor(j - 1) : {}">
                        <template v-if="i === activeLine">
                            <div class="peg" v-if="activeColors[j - 1] !== -1"
                                :style="{ background: `radial-gradient(circle at 30% 30%, #fff, ${customs.colors[activeColors[j - 1]]} 15%, #000)` }">
                            </div>
                            <div class="active blank" v-else></div>
                        </template>
                        <template v-else>
                            <div class="peg" v-if="i <= data.length && data[i - 1].slots[j - 1] !== -1"
                                :style="{ background: `radial-gradient(circle at 30% 30%, #fff, ${customs.colors[data[i - 1].slots[j - 1]]} 15%, #000)` }">
                            </div>
                            <div class="blank" v-else></div>
                        </template>
                    </div>
                </div>
            </div>
        </template>
    </div>
    <Pegs :customs="customs" :selectedColor="selectedColor" @setSelectedColor="setSelectedColor"
        @checkLine="checkLine" />
</template>

<style scoped>
.board {
    display: flex;
    padding: 2%;
    height: 100%;
    width: 65%;
    background: #cd8961;
    border-radius: 10px;
    justify-content: space-between;
}

.line {
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
}

.active.line {
    background: #ffaa78;
}

.pins {
    border: medium solid #5f2c14;
    display: flex;
    flex-wrap: wrap;
    width: 96%;
    justify-content: space-evenly;
    align-items: center;
}

.pin {
    aspect-ratio: 1;
    border-radius: 50%;
    width: 40%;
    box-shadow: 3px 3px 2px #00000080;
}

.exact {
    background: radial-gradient(circle at 30% 30%, #fff, #111 15%, #000);
}

.near {
    background: radial-gradient(circle at 30% 30%, #fff, #eee 15%, #000);
}

.blank {
    aspect-ratio: 1;
    border-radius: 50%;
    width: 34%;
    border: medium solid #5f2c14;
    background: #150b06;
    margin: 5% 0;
}

.slot .blank {
    width: 40%;
}

.slot-container {
    width: 100%;
    display: flex;
    justify-content: center;
}

.slot {
    display: grid;
    aspect-ratio: 1;
    width: 94%;
    border-radius: 50%;
    border: medium solid #5f2c14;
    justify-items: center;
    align-items: center;
    transition: all 0.2s;
}

.active.slot:hover {
    box-shadow: 0 0 3px 3px #fff;
}


.peg {
    aspect-ratio: 1;
    width: 80%;
    border-radius: 50%;
    background: radial-gradient(circle at 30% 30%, pink, #00000080);
    box-shadow: 6px 6px 4px 0 #00000080;
}
</style>