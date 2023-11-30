<script setup lang="ts">
import { ref } from 'vue';
import TheNav from '@/components/TheNav.vue';
import categories from '@/assets/categories';
const categries = ref(categories)
</script>
<template>
    <div class="wrapper">
        <the-nav />
        <van-index-bar :index-list="categries.map(item => item.title)">
            <div v-for="category in categries">
                <van-index-anchor class="category" :index="category.title">{{ category.title }}</van-index-anchor>
                <van-cell class="product" v-for="product in category.products" :title="product.name" />
            </div>
        </van-index-bar>
    </div>
</template>

<style scoped>
.wrapper {
    margin: auto;
    width: 1000px;
    height: 100%;
}

.category{
    color: whitesmoke;
}
.product{
    color: whitesmoke;
    background-color: #2D303E;
}
</style>






