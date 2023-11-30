<script setup lang="ts">
import TheNav from '@/components/TheNav.vue';
import { ref } from '@vue/reactivity';
import { Ref } from 'vue';
import { onMounted, onUnmounted } from 'vue';
import categories from '../assets/categories'
const divCategoriesBtns = ref()
const divCategoriesContents = ref()

const scrollPositionBtn = ref(0)
const scrollPositionContent = ref(0)

const scrollBtnIsActive = ref(true)
const scrollContentIsActive = ref(true)

const categries = ref(categories)


const onScroll = () => {
    scrollPositionContent.value = divCategoriesContents.value?.scrollTop ?? 0;
    setActiveCategory(scrollPositionContent.value)
};

const scrollBtnWheel = (event: any) => {
    if (!scrollBtnIsActive.value) return;
    const lastScrollPosition = scrollPositionBtn.value
    event.preventDefault();

    if (event.deltaY > 0) {
        scrollCategoryBtn(lastScrollPosition)
    } else {
        scrollCategoryBtn(lastScrollPosition, true)
    }
    scrollBtnIsActive.value = false;
    scrollPositionBtn.value = divCategoriesBtns.value?.scrollLeft ?? 0
    setTimeout(() => {
        scrollBtnIsActive.value = true;
    }, 40);
}

const setActiveCategory = (scroll: number) => {
    categries.value.forEach((item: { is_active: boolean; btnPostion: number; }, index: number) => {
        if (scroll < 16) {
            scroll = 16
        }
        if (scroll >= categries.value[index].contentPosition) {
            if (categries.value[index - 1]) {
                categries.value[index - 1].is_active = false
            }
            item.is_active = true
            divCategoriesBtns.value.scrollLeft = item.btnPostion - 50
        } else {
            item.is_active = false
        }
    });
}

const setBtnCategoryPositions = () => {
    const elements = divCategoriesBtns.value.querySelectorAll('.btn_category');
    elements.forEach((element: any, index: number) => {
        const position = getElementPosition(element, divCategoriesBtns);
        categries.value[index].btnPostion = position
    });
}
const setContentCategoryPosition = () => {
    const elements = divCategoriesContents.value.querySelectorAll('.category_block');
    elements.forEach((element: any, index: number) => {
        const position = element.offsetTop - divCategoriesContents.value.offsetTop;
        categries.value[index].contentPosition = position
    });
}
const getElementPosition = (element: { getBoundingClientRect: () => any; }, PareentElem: Ref<any>) => {
    const rect = element.getBoundingClientRect();
    const parentRect = PareentElem.value.getBoundingClientRect();
    return rect.left - parentRect.left;
};

const scrollContentCategoryBtn = (contentPosition: number, btnPostion: number) => {
    divCategoriesContents.value.scrollTop = contentPosition
    divCategoriesBtns.value.scrollLeft = btnPostion - 50
};

const scrollCategoryBtn = (value = divCategoriesBtns.value?.scrollLeft ?? 0, is_left = false) => {
    const index = categries.value.findIndex((item: { btnPostion: number; }) => item.btnPostion >= value)
    if (is_left && index && index != -1) {
        divCategoriesBtns.value.scrollLeft = categries.value[index - 1].btnPostion - 50
    } else if (!is_left && index != -1) {
        divCategoriesBtns.value.scrollLeft = categries.value[index + 1].btnPostion - 50
    }
}
onMounted(() => {
    setBtnCategoryPositions()
    setContentCategoryPosition()
    setActiveCategory(divCategoriesContents.value?.scrollLeft ?? 0)
})
</script>
<template>
    <div class="wrapper">
        <the-nav />
        <div class="categories_header_wrapper">
            <div>
                <div @click.stop.prevent="scrollCategoryBtn(undefined, true)" class="btn_scroll">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                        class="bi bi-chevron-left" viewBox="0 0 16 16">
                        <path fill-rule="evenodd"
                            d="M11.354 1.646a.5.5 0 0 1 0 .708L5.707 8l5.647 5.646a.5.5 0 0 1-.708.708l-6-6a.5.5 0 0 1 0-.708l6-6a.5.5 0 0 1 .708 0z" />
                    </svg>
                </div>
            </div>
            <div ref="divCategoriesBtns" @wheel="scrollBtnWheel" class="categories_btns">
                <div v-for="categryBtn in categries"
                    @click.stop.prevent="scrollContentCategoryBtn(categryBtn.contentPosition, categryBtn.btnPostion)"
                    class="btn_category" :class="{ 'btn_category_active': categryBtn.is_active }">
                    {{ categryBtn.title }}</div>
            </div>
            <div>
                <div @click.stop.prevent="scrollCategoryBtn()" class="btn_scroll">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                        class="bi bi-chevron-right" viewBox="0 0 16 16">
                        <path fill-rule="evenodd"
                            d="M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z" />
                    </svg>
                </div>
            </div>
        </div>
        <div class="categories_blocks_wrapper">
            <div class="categories_blocks" ref="divCategoriesContents" @scroll="onScroll">
                <div v-for="categry in categries">
                    <div class="category_block_wrapper">
                        <div class="category_block">
                            <div class="category_block_main_text">
                                <p>
                                    {{ categry.title }}
                                </p>
                            </div>
                            <div class="products_wrapper">
                                <div v-for="product in categry.products" class="category_products">
                                    <p>{{ product.name }}</p>
                                    <p>{{ product.description }}</p>
                                    <p>{{ product.price }}</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.btn_category_active {
    background-color: #76B900;
}

.wrapper {
    display: flex;
    flex-direction: column;
    margin: auto;
    width: 1000px;
    height: 100%;
}

.categories_header_wrapper {
    box-sizing: border-box;
    /* width: 600px; */
    width: 100%;
    /* box-shadow: 0px 0px 4px #76B900; */
    border-radius: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
    margin: auto;
    margin-top: 1rem;
}

.categories_btns {
    overflow: hidden;
    display: flex;
    width: 100%;
}

.categories_btns::-webkit-scrollbar {
    display: none;
}

.btn_category {
    cursor: pointer;
    padding: 0.5rem;
    border-radius: 1rem;
    margin: 0.5rem 0.3rem;
    white-space: nowrap;
    /* box-shadow: 0px 0px 4px rgb(200, 200, 200); */
}

.btn_scroll {
    cursor: pointer;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 30px;
    height: 30px;
    margin: 0px 10px;
    background-color: #76B900;
    /* box-shadow: 0px 0px 4px rgb(200, 200, 200); */
}

.btn_scroll:hover {
    background-color: #16141f;
    /* background-color: rgb(40, 40, 40); */
}

.btn_category:hover {
    background-color: #16141f;
    /* background-color: rgb(202, 255, 196); */
}

.categories_blocks_wrapper {
    margin: 1rem 0rem;
    padding: 14px 0px;
    /* border-radius: 1rem; */
    /* box-shadow: 0px 0px 4px rgb(200, 200, 200); */
    width: 100%;
    height: 100%;
    display: flex;
    overflow: hidden;
    flex-direction: column;
    box-sizing: border-box;

}


.categories_blocks {
    height: 100%;
    overflow: auto;
}

.categories_blocks::-webkit-scrollbar {
    width: 4px;
    height: 4px;
}

.categories_blocks::-webkit-scrollbar-track {
    /* box-shadow: inset 0 0 5px rgb(255, 255, 255); */
    background-color: #16141f;
    border-radius: 10px;
}

.categories_blocks::-webkit-scrollbar-thumb {
    background: #76B900;
    border-radius: 10px;
}


.category_block_wrapper {
    padding: 1rem;
}

.category_block {
    border-radius: 1rem;
    box-sizing: border-box;
    padding: 1rem 0rem;
    text-align: center;
    box-sizing: border-box;
    /* height: 600px; */
    width: 100%;
    /* box-shadow: 0px 0px 4px rgb(200, 200, 200); */
    display: flex;
    flex-direction: column;
}

.category_block_main_text {
    font-size: 20px;
    font-weight: 600;
}

.products_wrapper {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    height: 100%;
}

.category_products {
    background-color: #2D303E;
    margin: 10px;
    border-radius: 1rem;
    flex: 1 1 200px;
    /* set a fixed width for each product */
    /* box-shadow: 0px 0px 4px rgb(200, 200, 200); */
}
</style>