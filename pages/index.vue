<template>
    <div>
        <!-- Search input and submit button -->
        <div class="d-flex flex-lg-row justify-content-center align-items-center flex-column p-5 gap">
            <input type="search" placeholder="Search the recipe" class="search-bar w-75 p-2 border border-black border-1 rounded" v-model="input" />
            <!-- <button class="btn btn-success fs-5" @click="searchRecipes">
                        Search
                    </button> -->
        </div>
        <div v-if="!issearch">
    
            <random/>
    
        </div>
        <div v-else>
    
            <search-recipe :input="input" />
    
        </div>
    </div>
</template>

<script setup>
const input = ref('');
const issearch = ref(false)

console.log('Index rendered');


onUpdated(() => {
    if (input.value !== '') {
        issearch.value = true;
    } else {
        issearch.value = false;
    }
})
</script>

<style scoped>

</style>