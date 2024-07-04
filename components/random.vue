<template>
    <div>
        <h3 class="splide-title text-center">Trending...</h3>
        <div v-if="isLoading">
            <div class="d-flex justify-content-center align-items-center">
                <div class="spinner-border mt-5 fs-3 " style="width: 3rem; height: 3rem;" role="status">
                    <span class="visually-hidden">Loading...</span>
                </div>
            </div>
        </div>
        <div v-else>
            <div class="splide" ref="splideRef">
                <div class="splide__track">
                    <ul class="splide__list food-list">
                        <li v-for="recipe in recipes" :key="recipe.idMeal" class="splide__slide card-container" @click="openRecipeDetails(recipe)" >
                            <div class="card" >
                                <img :src="recipe.strMealThumb" alt="" class="card-img-top">
                                <div class="card-body">
                                    <p class="card-title">{{ recipe.strMeal }}</p>
                                    <p class="card-text">It's a {{ recipe.strArea }} dish</p>
                                </div>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
        <recipedetails v-if="isVisible" :recipe="selectedRecipe" @close="closeRecipeDetails" />
        <h3 class="text-center" style=" position:absolute;bottom:30px; left: 50% ; transform: translateX(-50%) ">Search <span class="text-success fw-bold">Recipe!</span> Serve <span class="text-success fw-bold">Happy !!</span></h3>
    </div>
</template>

<script setup>
import { ref, onMounted, reactive } from 'vue';
import axios from 'axios';
import Splide from '@splidejs/splide';

const recipes = reactive([]);
const isLoading = ref(true);
const isError = ref(false)
const isVisible = ref(false)
const selectedRecipe = ref([]);

console.log('Random rendered');

const getRandomRecipe = async () => {
    try {
        for (let i = 0; i < 12; i++) {
            const response = await axios.get('https://www.themealdb.com/api/json/v1/1/random.php');
            const data = response.data;
            if (data && Array.isArray(data.meals)) {
                recipes.push(data.meals[0]);
            } else {
                console.log('error at random');
                return isError.value = false;
            }
        }
    } catch (error) {
        console.error("Error fetching random recipes:", error);
    } finally {
        isLoading.value = false
    }
};

function openRecipeDetails(recipe) {
    console.log('Opening recipe details:', recipe);
    selectedRecipe.value = recipe;
    isVisible.value = true;
}

function closeRecipeDetails() {
    console.log('Closing recipe details');
    isVisible.value = false;
    selectedRecipe.value = null;
}

onMounted(async () => {
    await getRandomRecipe();

    const splide = new Splide('.splide', {
        type: 'loop',
        drag: 'free',
        focus: 'center',
        perPage: 3,
        gap: '1rem',
        pagination: false,
        arrows: false,
        autoScroll: {
            speed: 2,
            pauseOnHover: true,
            pauseOnFocus: true,
        },
    });

    splide.mount();

    setInterval(() => {
        splide.go('+1');
    }, 2000);
});
</script>

<style scoped>
.slider-container {
    overflow-x: scroll;
    white-space: nowrap;
    padding: 0.5rem;
}

.splide-title {
    color: green;
    font-weight: 700;
}

.card-container {
    display: inline-block;
    margin: 0 -1rem;
}

.card {
    width: 18rem;
}

.card-img-top {
    width: 100%;
    /* height: auto; */
}

.card-body {
    background-color: #fff;
    padding: 1rem;
    border: 1px solid #ccc;
    border-top: none;
    text-align: center;
}

.card-title {
    font-weight: bold;
    font-size: 1.2rem;
    margin-bottom: 0.5rem;
}

.card-title,
.text-wrap {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 100%;
}

.card-text {
    font-size: 1rem;
    color: #555;
}

.gap {
    gap: 10px;
}

.splide__slide {
    display: flex;
    justify-content: center;
}

.splide {
    display: flex;
    justify-content: center;
    align-content: center;
    width: 70%;
    flex-direction: column;
    margin: 0 auto;
    padding: 20px 0;
}
</style>
