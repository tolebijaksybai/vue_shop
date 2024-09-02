<script setup>
import { onMounted, provide, reactive, ref, watch } from 'vue'
import axios from 'axios'

import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import Drawer from '@/components/Drawer.vue'

const items = ref([])
const filters = reactive({
    sortBy: 'title',
    searchQuery: ''
})
const drawerOpen = ref(false)
const cart = ref([])

const onChangeSelect = event => {
    filters.sortBy = event.target.value
}

const onChangeSearchInput = event => {
    filters.searchQuery = event.target.value
}

const closeDrawer = () => {
    drawerOpen.value = false
}

const openDrawer = () => {
    drawerOpen.value = true
}

const fetchFavorites = async () => {
    try {
        const { data: favorites } = await axios.get(`https://ec0774525fb6695f.mokky.dev/favorites`)

        items.value = items.value.map(item => {
            const favorite = favorites.find(favorite => favorite.productId === item.id)

            if (!favorite) {
                return item
            }

            return {
                ...item,
                isFavorite: true,
                favoriteId: favorite.id
            }
        })
    } catch (err) {
        console.log(err)
    }
}

const fetchItems = async () => {
    try {
        const params = {
            sortBy: filters.sortBy
        }

        if (filters.searchQuery) {
            params.title = `*${filters.searchQuery}*`
        }

        const { data } = await axios.get(`https://ec0774525fb6695f.mokky.dev/items`, { params })

        items.value = data.map((object) => ({
            ...object,
            isFavorite: false,
            favoriteId: null,
            isAdded: false
        }))
    } catch (err) {
        console.log(err)
    }
}

const addToFavorite = async (item) => {
    try {
        if (!item.isFavorite) {
            item.isFavorite = true

            const object = {
                productId: item.id
            }

            const { data } = await axios.post(`https://ec0774525fb6695f.mokky.dev/favorites`, object)

            item.favoriteId = data.id
        } else {
            item.isFavorite = false

            await axios.delete(`https://ec0774525fb6695f.mokky.dev/favorites/${item.favoriteId}`)

            item.favoriteId = null
        }
    } catch (err) {
        console.log(err)
    }
}

const addToCart = (item) => {
    if (!item.isAdded) {
        cart.value.push(item)
        item.isAdded = true
    } else {
        cart.value.splice(cart.value.indexOf(item), 1)
        item.isAdded = false
    }
}

onMounted(async () => {
    await fetchItems()
    await fetchFavorites()
})

watch(filters, fetchItems)

provide('cart', {
    cart,
    closeDrawer,
    openDrawer
})
</script>

<template>
    <Drawer v-if="drawerOpen" />

    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 mb-14">
        <Header @open-drawer="openDrawer" />

        <div class="p-10">
            <div class="flex justify-between items-center mb-8">
                <h2 class="text-3xl font-bold">Все кроссовки</h2>

                <div class="flex gap-1.5 ml-auto flex-wrap">
                    <select
                        @change="onChangeSelect"
                        class="py-2 px-3 border rounded-md outline-none">
                        <option value="title">По названию</option>
                        <option value="price">По цене (дешевые)</option>
                        <option value="-price">По цене (дорогие)</option>
                    </select>

                    <div class="relative ml-2">
                        <img src="/img/search.svg" alt="Search" class="absolute left-4 top-3">
                        <input
                            @input="onChangeSearchInput"
                            class="border border-gray-200 rounded-md py-1.5 pl-11 p-10 pr-4 outline-none focus:border-gray-400"
                            placeholder="Поиск ..." />
                    </div>
                </div>
            </div>

            <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="addToCart" />

        </div>
    </div>
</template>

