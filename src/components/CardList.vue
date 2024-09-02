<script setup>
import Card from './Card.vue'

const props = defineProps({
    items: Array
})

const emit = defineEmits(['addToFavorite'])

</script>

<template>
    <div class="grid grid-cols-4 gap-5">
        <Card
            v-for="item in props.items"
            :key="item.id"
            :id="item.id"
            :title="item.title"
            :imageUrl="'/img/' + item.imageUrl"
            :price="Number(item.price)"
            :isFavorite="item.isFavorite"
            :isAdded="item.isAdded"

            :onClickFavorite="() => emit('addToFavorite', item)"
        />
    </div>
</template>
