<script setup lang="ts">
/**
 * Abacus
 *
 * @link        https://abacus.afaan.dev
 *
 * @author      Afaan Bilal
 * @link        https://afaan.dev
 * @link        https://github.com/AfaanBilal/abacus
 * @license     MIT
 * @copyright   2024 Afaan Bilal
 */

import { ref, watch } from 'vue';

type EmptySpot = { top: boolean, bottom: number };

const COUNT = Math.ceil(Math.max(4, Math.min(13, window.innerWidth / 80)));
const MAX = Number('9'.repeat(COUNT));

const resetState = (to: number = 0) => {
    const data: Array<EmptySpot> = [];

    for (let i = 0; i < COUNT; i++) {
        const digit = Number(to.toString().padStart(COUNT, '0').split('')[i]);
        data.push({ top: digit >= 5, bottom: digit >= 5 ? digit - 5 : digit });
    }

    return data;
};

const calculateValue = () => {
    let v = 0;

    for (let i = 0; i < COUNT; i++) {
        v += Math.pow(10, COUNT - i - 1) * (emptySpots.value[i].bottom + (emptySpots.value[i].top ? 5 : 0));
    }

    value.value = v;
};

const value = ref(0);
const emptySpots = ref<Array<EmptySpot>>(resetState());
const autoplay = ref(false);

watch(value, () => {
    if (value.value > MAX) {
        value.value = MAX;
    }

    emptySpots.value = resetState(value.value);
});

const moveTopSpot = (i: number) => {
    emptySpots.value[i].top = !emptySpots.value[i].top;
    calculateValue();
};

const moveSpot = (i: number, v: number) => {
    emptySpots.value[i].bottom = v;
    calculateValue();
};

const reset = () => {
    emptySpots.value = resetState();
    calculateValue();
};

setInterval(() => {
    if (autoplay.value) {
        if (value.value < MAX) {
            value.value += 1;
        }
    }
}, 700);
</script>

<template>
    <div class="flex justify-center">
        <div class="flex flex-col max-w-full gap-4 p-4 m-2 overflow-x-auto bg-white rounded-lg">
            <div class="flex items-center gap-4 mb-4">
                <div class="flex-1 font-mono text-3xl font-extrabold text-gray-800">ABACUS</div>
                <div class="flex items-center gap-2">
                    <input type="checkbox" v-model="autoplay" class="w-4 h-4 cursor-pointer accent-red-900" id="autoplay" />
                    <label for="autoplay" class="cursor-pointer select-none">Autoplay</label>
                </div>
                <div class="flex items-center justify-center p-1 px-2 text-xl text-gray-800 bg-gray-300 rounded-lg cursor-pointer select-none hover:bg-gray-200 active:bg-gray-400"
                    @click="reset">
                    Clear
                </div>
            </div>
            <div class="flex items-center gap-2 pb-4 border-b">
                <div @click="() => { if (value < MAX) { value += 1; }; }"
                    class="flex items-center self-stretch justify-center w-10 text-gray-800 bg-gray-300 rounded cursor-pointer select-none hover:bg-gray-200 active:bg-gray-400">
                    &#8613;
                </div>
                <div class="flex-1">
                    <input type="number" v-model="value" min="0" :max="MAX"
                        class="p-1 px-4 font-mono w-full text-3xl text-right text-gray-200 bg-gray-800 rounded-lg [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none" />
                </div>
                <div @click="() => { if (value > 0) { value -= 1; }; }"
                    class="flex items-center self-stretch justify-center w-10 text-gray-800 bg-gray-300 rounded cursor-pointer select-none hover:bg-gray-200 active:bg-gray-400">
                    &#8615;
                </div>
            </div>
            <div class="flex gap-4 pb-4 mb-2 border-b-2 border-b-gray-300">
                <div v-for="i in COUNT" class="flex flex-col gap-2">
                    <div class="w-12 h-12 transition-colors cursor-pointer rounded-xl hover:border-2"
                        @click="moveTopSpot(i - 1)"
                        :class="{ 'bg-red-900': !emptySpots[i - 1].top, 'bg-gray-200': emptySpots[i - 1].top }">
                    </div>
                    <div class="w-12 h-12 transition-colors cursor-pointer rounded-xl hover:border-2"
                        @click="moveTopSpot(i - 1)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].top, 'bg-gray-200': !emptySpots[i - 1].top }">
                    </div>
                </div>
            </div>
            <div class="flex gap-4">
                <div v-for="i in COUNT" class="flex flex-col gap-2">
                    <div class="w-12 h-12 transition-colors cursor-pointer rounded-xl hover:border-2"
                        @click="moveSpot(i - 1, 0)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 0, 'bg-gray-200': emptySpots[i - 1].bottom === 0 }">
                    </div>
                    <div class="w-12 h-12 transition-colors cursor-pointer rounded-xl hover:border-2"
                        @click="moveSpot(i - 1, 1)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 1, 'bg-gray-200': emptySpots[i - 1].bottom === 1 }">
                    </div>
                    <div class="w-12 h-12 transition-colors cursor-pointer rounded-xl hover:border-2"
                        @click="moveSpot(i - 1, 2)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 2, 'bg-gray-200': emptySpots[i - 1].bottom === 2 }">
                    </div>
                    <div class="w-12 h-12 transition-colors cursor-pointer rounded-xl hover:border-2"
                        @click="moveSpot(i - 1, 3)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 3, 'bg-gray-200': emptySpots[i - 1].bottom === 3 }">
                    </div>
                    <div class="w-12 h-12 transition-colors cursor-pointer rounded-xl hover:border-2"
                        @click="moveSpot(i - 1, 4)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 4, 'bg-gray-200': emptySpots[i - 1].bottom === 4 }">
                    </div>
                </div>
            </div>
            <div class="flex items-center justify-end gap-2 p-2 text-sm border-t">
                <a href="https://github.com/AfaanBilal/abacus" target="_blank" rel="noopener" class="hover:text-blue-500">
                    GitHub
                </a>
                &middot;
                <a href="https://afaan.dev" target="_blank" rel="noopener" class="hover:text-blue-500">
                    Copyright &copy; Afaan Bilal
                </a>
            </div>
        </div>
    </div>
</template>
