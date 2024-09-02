<script setup>
import { inject } from 'vue'

import DrawerHead from './DrawerHead.vue'
import CartItemList from '@/components/CartItemList.vue'

const { closeDrawer } = inject('cart')
</script>

<template>
    <div @click="closeDrawer" class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70 transition"></div>
    <div class="flex flex-col bg-white w-96 h-full fixed right-0 top-0 z-20 p-10 transition">
        <DrawerHead />

        <CartItemList />

        <div class="flex flex-col gap-4 mt-7">
            <div class="flex gap-2 justify-between">
                <span>Итого: </span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>21 498 руб. </b>
            </div>

            <div class="flex gap-2 justify-between">
                <span>Налог 5%: </span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>1074 руб. </b>
            </div>

            <button
                disabled
                class="bg-lime-500 mt-4 w-full rounded-xl py-3 text-white hover:bg-lime-600 transition active:bg-lime-700 disabled:bg-slate-300 cursor-pointer">
                Оформить заказ
            </button>
        </div>

    </div>
</template>
