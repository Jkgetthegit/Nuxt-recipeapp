<template>
    <div>
        <h2 class="fw-bold text-center">
            Here is the search Result <span class="text-danger">"{{ props.input }}"</span>
        </h2>
        <div v-if="isLoading">
            <div class="d-flex justify-content-center align-items-center">
                <div class="spinner-border mt-5 fs-3 " style="width: 3rem; height: 3rem;" role="status">
                    <span class="visually-hidden">Loading...</span>
                </div>
            </div>
        </div>
        <!-- Display the recipe -->
        <div v-else>
            <div class="d-flex justify-content-center align-items-center flex-wrap mt-3">
                <div v-if="currentRecipes.length > 0">
                    <div v-for="(recipe, index) in currentRecipes" :key="index" class="card-container" @click="openRecipeDetails(recipe)">
                        <div class="card rounded">
                            <img :src="recipe.strMealThumb" class="card-img-top" :alt="recipe.strMeal" />
                            <div class="card-body text-center">
                                <h5 class="card-title text-truncate">{{ recipe.strMeal }}</h5>
                                <p class="card-text"><i>It's a {{ recipe.strArea }} dish</i></p>
                            </div>
                        </div>
                    </div>
                </div>
                <p v-else class="text-center fs-2 fw-bold text-capitalize mt-5">
                    <span class="text-danger">OOPS!</span> No recipes found.
                </p>
            </div>
    
            <!-- Pagination controls -->
            <nav aria-label="Page navigation">
                <ul class="pagination justify-content-center">
                    <li class="page-item" :class="{ disabled: currentPage === 1 }">
                        <button class="page-link" @click="goToPreviousPage"><IconsLeftArrow/></button>
                    </li>
                    <li v-for="page in totalPages" :key="page" class="page-item" :class="{ active: currentPage === page }">
                        <button class="page-link" @click="goToPage(page)">{{ page }}</button>
                    </li>
                    <li class="page-item" :class="{ disabled: currentPage === totalPages }">
                        <button class="page-link" @click="goToNextPage"><IconsRightArrow/></button>
                    </li>
                </ul>
            </nav>
            <!-- Recipe Details Modal -->
            <recipedetails v-if="isVisible" :recipe="selectedRecipe" @close="closeRecipeDetails" />
        </div>
    </div>
</template>
  
<script setup>
// Define props for input
const props = defineProps({
    input: {
        type: String,
        required: true
    }
});
const currentPage = ref(1);
const itemsPerPage = ref(3);
const searchedRecipes = ref([]);
const selectedRecipe = ref([]);
const isVisible = ref(false)
const isLoading = ref(true)

// Methods to open the recipeDetails components
function openRecipeDetails(recipe) {
    // console.log('Opening recipe details:', recipe);
    selectedRecipe.value = recipe;
    isVisible.value = true;
}

function closeRecipeDetails() {
    // console.log('Closing recipe details');
    isVisible.value = false;
    selectedRecipe.value = null;
}

// Fetch recipes based on input
const showSearchRecipe = async () => {
    try {
        const api = await fetch(
            `https://www.themealdb.com/api/json/v1/1/search.php?s=${props.input}`
        );
        const fetchedData = await api.json();
        searchedRecipes.value = fetchedData.meals || [];
    } catch (error) {
        console.error("Error fetching data:", error);
    } finally {
        isLoading.value = false;
    }
};
console.log('search recipe rendered');

// Load recipes on component mount
onMounted(() => {
    if (props.input) {
        showSearchRecipe();
    }
});

// showSearchRecipe()
onUpdated(() => {
    showSearchRecipe();
})


// Computed property for paginated recipes
const currentRecipes = computed(() => {
    const start = (currentPage.value - 1) * itemsPerPage.value;
    const end = start + itemsPerPage.value;
    return searchedRecipes.value.slice(start, end);
});

// Computed property for total pages
const totalPages = computed(() => {
    return Math.ceil(searchedRecipes.value.length / itemsPerPage.value);
});

// Methods to navigate pages
const goToPage = (page) => {
    if (page >= 1 && page <= totalPages.value) {
        currentPage.value = page;
    }
};

//methods for gotopreviouspage in pagination
const goToPreviousPage = () => {
    if (currentPage.value > 1) {
        currentPage.value--;
    }
};

//methods for gotonextpage in pagination
const goToNextPage = () => {
    if (currentPage.value < totalPages.value) {
        currentPage.value++;
    }
};
</script>
  
<style scoped>
.card-container {
    display: inline-block;
    padding: 10px;
}

.card {
    width: 18rem;
    border: 1px solid #ccc;
    border-radius: 8px;
    overflow: hidden;
    transition: transform 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.card-img-top {
    width: 100%;
    height: 200px;
    object-fit: cover;
}

.card-body {
    padding: 1rem;
    background-color: #fff;
}

.card-title {
    font-weight: bold;
    font-size: 1.2rem;
    margin-bottom: 0.5rem;
}

.card-text {
    font-size: 1rem;
    color: #555;
}
</style>
  