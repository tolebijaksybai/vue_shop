<script setup>
defineProps({
    id: Number,
    imageUrl: String,
    title: String,
    price: Number
})
</script>
<template>
    <div class="flex items-center gap-4 w-full border border-slate-100 p-4 rounded-xl">
        <img :src="imageUrl" alt="Sneaker" class="w-16 h-16">

        <div class="flex justify-between flex-col gap-2">
            <p>{{ title }}</p>
            <div class="flex justify-between">
                <b>{{ price }} руб.</b>
                <img src="/img/close.svg" alt="Close" class="opacity-30 hover:opacity-100 cursor-pointer transition">
            </div>
        </div>
    </div>
</template>
