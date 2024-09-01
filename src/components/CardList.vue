<script setup>
import Card from './Card.vue'
import Drawer from '@/components/Drawer.vue'

const onClickAdd = () => {
    alert('Добавить корзину')
}
</script>

<template>
    <div class="grid grid-cols-4 gap-5">
        <Card
            title="Мужские Кроссовки Nike Blazer Mid Suede"
            imageUrl="/img/sneakers/sneakers-1.jpg"
            :price="5000"
            :isFavorite="true"
            :isAdded="false"
            :onClickAdd="onClickAdd"
        />
    </div>

    <Drawer />
</template>
