<script setup lang="ts">
import { ref } from 'vue';

const categries = ref([
    {
        id: 1, title: "category1", products: [
            { title: "product1" },
            { title: "product2" },
            { title: "product3" },
        ]
    },
    {
        id: 2, title: "category2", products: [
            { title: "product1" },
            { title: "product2" },
            { title: "product3" },
        ]
    },
    {
        id: 3, title: "category3", products: [
            { title: "product1" },
            { title: "product2" },
        ]
    },
    {
        id: 4, title: "category4", products: [
            { title: "product1" },
            { title: "product2" },
            { title: "product3" },
        ]
    },
    {
        id: 5, title: "category5", products: [
            { title: "product1" },
            { title: "product2" },
            { title: "product3" },
        ]
    },
    {
        id: 6, title: "category6", products: [
            { title: "product1" },
            { title: "product2" },
            { title: "product3" },
        ]
    },
    {
        id: 7, title: "category7", products: [
            { title: "product1" },
            { title: "product2" },
            { title: "product3" },
        ]
    },
    {
        id: 8, title: "category8", products: [
            { title: "product1" },
            { title: "product2" },
            { title: "product3" },
        ]
    },
    {
        id: 9, title: "category9", products: [
            { title: "product1" },
            { title: "product2" },
            { title: "product3" },
        ]
    },
    {
        id: 10, title: "category10", products: [
            { title: "product1" },
            { title: "product2" },
            { title: "product3" },
        ]
    },
])
</script>
<template>
    <div>
        <the-nav />
        <van-index-bar :index-list="categries.map(item => item.title)">
            <div v-for="item in categries">
                <van-index-anchor :index="item.title">{{ item.title }}</van-index-anchor>
                <van-cell v-for="value in item.products" :title="value.title" />
            </div>
        </van-index-bar>
    </div>
</template>

<style scoped></style>






