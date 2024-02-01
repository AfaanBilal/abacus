<script setup lang="ts">
import { ref } from 'vue';

type EmptySpot = { top: boolean, bottom: number };

const resetState = () => {
    const data: Array<EmptySpot> = [];

    for (let i = 0; i < 13; i++) {
        data.push({ top: false, bottom: 0 });
    }

    return data;
};

const emptySpots = ref<Array<EmptySpot>>(resetState());

const moveTopSpot = (i: number) => emptySpots.value[i].top = !emptySpots.value[i].top;
const moveSpot = (i: number, v: number) => emptySpots.value[i].bottom = v;

const reset = () => emptySpots.value = resetState();
</script>

<template>
    <div class="flex justify-center">
        <div class="flex flex-col gap-2 p-4 m-4 border rounded">
            <div class="flex items-center justify-between p-2 pb-4 mb-4 border-b">
                <div class="font-mono text-3xl text-center">ABACUS</div>
                <div class="p-1 px-2 text-white bg-gray-800 rounded cursor-pointer" @click="reset">Reset</div>
            </div>
            <div class="flex gap-4 p-2 pb-4 mb-2 border-b-2 border-b-gray-300">
                <div v-for="i in 13" class="flex flex-col gap-2">
                    <div class="w-16 h-16 rounded-2xl" @click="moveTopSpot(i - 1)"
                        :class="{ 'bg-red-900': !emptySpots[i - 1].top, 'bg-gray-200': emptySpots[i - 1].top }">
                    </div>
                    <div class="w-16 h-16 rounded-2xl" @click="moveTopSpot(i - 1)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].top, 'bg-gray-200': !emptySpots[i - 1].top }">
                    </div>
                </div>
            </div>
            <div class="flex gap-4 p-2">
                <div v-for="i in 13" class="flex flex-col gap-2">
                    <div class="w-16 h-16 rounded-2xl" @click="moveSpot(i - 1, 0)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 0, 'bg-gray-200': emptySpots[i - 1].bottom === 0 }">
                    </div>
                    <div class="w-16 h-16 rounded-2xl" @click="moveSpot(i - 1, 1)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 1, 'bg-gray-200': emptySpots[i - 1].bottom === 1 }">
                    </div>
                    <div class="w-16 h-16 rounded-2xl" @click="moveSpot(i - 1, 2)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 2, 'bg-gray-200': emptySpots[i - 1].bottom === 2 }">
                    </div>
                    <div class="w-16 h-16 rounded-2xl" @click="moveSpot(i - 1, 3)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 3, 'bg-gray-200': emptySpots[i - 1].bottom === 3 }">
                    </div>
                    <div class="w-16 h-16 rounded-2xl" @click="moveSpot(i - 1, 4)"
                        :class="{ 'bg-red-900': emptySpots[i - 1].bottom !== 4, 'bg-gray-200': emptySpots[i - 1].bottom === 4 }">
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
