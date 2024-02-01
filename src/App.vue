<script setup lang="ts">
import { computed, ref } from 'vue';

type EmptySpot = { top: boolean, bottom: number };

const resetState = () => {
    const data: Array<EmptySpot> = [];

    for (let i = 0; i < 13; i++) {
        data.push({ top: false, bottom: 0 });
    }

    return data;
};

const emptySpots = ref<Array<EmptySpot>>(resetState());

const value = computed(() => {
    let v = 0;

    for (let i = 0; i < 13; i++) {
        v += Math.pow(10, 13 - i - 1) * (emptySpots.value[i].bottom + (emptySpots.value[i].top ? 5 : 0));
    }

    return v;
});

const moveTopSpot = (i: number) => emptySpots.value[i].top = !emptySpots.value[i].top;
const moveSpot = (i: number, v: number) => emptySpots.value[i].bottom = v;

const reset = () => emptySpots.value = resetState();
</script>

<template>
    <div class="flex justify-center">
        <div class="flex flex-col gap-2 p-4 m-4 bg-white rounded-lg">
            <div class="flex items-center gap-4 p-2 mb-4 border-b">
                <div class="flex-1 font-mono text-3xl font-extrabold text-gray-800">ABACUS</div>
                <div class="flex-1 p-1 px-4 font-mono text-2xl text-right text-gray-200 bg-gray-800 rounded-lg">
                    {{ value }}
                </div>
                <div class="flex items-center justify-center h-full p-1 px-2 text-xl text-gray-800 bg-gray-300 rounded-lg cursor-pointer select-none hover:bg-gray-200 active:bg-gray-400"
                    @click="reset">
                    Clear
                </div>
            </div>
            <div class="flex gap-4 p-2 pb-4 mb-2 border-b-2 border-b-gray-300">
                <div v-for="i in 13" class="flex flex-col gap-2">
                    <div class="w-16 h-16 cursor-pointer rounded-2xl hover:border-2" @click="moveTopSpot(i - 1)"
                        :class="{ 'bg-red-900': !emptySpots[i - 1].top, 'bg-gray-200': emptySpots[i - 1].top }">
                    </div>
                    <div class="w-16 h-16 cursor-pointer rounded-2xl hover:border-2" @click="moveTopSpot(i - 1)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].top, 'bg-gray-200': !emptySpots[i - 1].top }">
                    </div>
                </div>
            </div>
            <div class="flex gap-4 p-2">
                <div v-for="i in 13" class="flex flex-col gap-2">
                    <div class="w-16 h-16 cursor-pointer rounded-2xl hover:border-2" @click="moveSpot(i - 1, 0)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 0, 'bg-gray-200': emptySpots[i - 1].bottom === 0 }">
                    </div>
                    <div class="w-16 h-16 cursor-pointer rounded-2xl hover:border-2" @click="moveSpot(i - 1, 1)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 1, 'bg-gray-200': emptySpots[i - 1].bottom === 1 }">
                    </div>
                    <div class="w-16 h-16 cursor-pointer rounded-2xl hover:border-2" @click="moveSpot(i - 1, 2)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 2, 'bg-gray-200': emptySpots[i - 1].bottom === 2 }">
                    </div>
                    <div class="w-16 h-16 cursor-pointer rounded-2xl hover:border-2" @click="moveSpot(i - 1, 3)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 3, 'bg-gray-200': emptySpots[i - 1].bottom === 3 }">
                    </div>
                    <div class="w-16 h-16 cursor-pointer rounded-2xl hover:border-2" @click="moveSpot(i - 1, 4)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 4, 'bg-gray-200': emptySpots[i - 1].bottom === 4 }">
                    </div>
                </div>
            </div>
            <div class="flex items-center justify-end pt-2 text-sm border-t hover:text-blue-500">
                <a href="https://afaan.dev" target="_blank" rel="noopener">Copyright &copy; Afaan Bilal</a>
            </div>
        </div>
    </div>
</template>
