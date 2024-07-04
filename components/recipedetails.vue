<template>
  <div class="popup">
      <div class="card">
          <div class="card-nav">
              <h3 class="card-header border-0 bg-white">{{ recipe.strMeal }}</h3>
              <button class="border-0 bg-white me-2" @click="close">&#10060;</button>
          </div>
          <div class="card-body">
              <a href="#" class="btn btn-primary me-2" @click="handleChangeContent('instructions')">Instructions</a>
              <a href="#" class="btn btn-danger" @click="handleChangeContent('ingredients')">Ingredients</a>
              <h5 class="card-title"></h5>
              <div v-if="isInstructions">
                  <p class="card-text">{{ recipe.strInstructions }}</p>
              </div>
              <div v-else>
                  <ul>
                      <li v-for="(item, index) in ingredientsAndMeasurements" :key="index">
                          {{ item.ingredient }} - {{ item.measure }}
                      </li>
                  </ul>
              </div>
          </div>
      </div>
  </div>
</template>

<script setup>

const props = defineProps({
  recipe: {
      type: Object,
      required: true
  }
});

const emit = defineEmits(['close']);
const isInstructions = ref(true);


const close = () => {
  emit('close');
};

const getIngredientsAndMeasurements = (recipe) => {
  const ingredients = [];
  for (let i = 1; i <= 20; i++) {
      const ingredient = recipe[`strIngredient${i}`];
      const measure = recipe[`strMeasure${i}`];
      if (ingredient && ingredient.trim() && measure && measure.trim()) {
          ingredients.push({ ingredient, measure });
      }
  }
  console.log(ingredients);
  return ingredients;
};

const ingredientsAndMeasurements = getIngredientsAndMeasurements(props.recipe);

const handleChangeContent = (content) => {
  isInstructions.value = content === 'instructions';
  console.log(isInstructions.value);
};
</script>

<style scoped>
.popup {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1000;
}

.card {
  width: 50%;
}

.card-nav {
  display: flex;
  justify-content: space-between;
}
</style>
