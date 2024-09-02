<script setup>
import { inject } from 'vue'

import CartItem from '@/components/CartItem.vue'

const { cart } = inject('cart')
</script>

<template>
    <div class="flex flex-col gap-4 flex-1">
        <CartItem
            v-for="item in cart"
            :key="item.id"
            :id="item.id"
            :title="item.title"
            :imageUrl="'/img/' + item.imageUrl"
            :price="Number(item.price)"
        />
    </div>
</template>
