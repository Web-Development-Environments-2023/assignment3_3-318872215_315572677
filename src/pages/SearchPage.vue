<template>
  <div class="container">

    <!-- maybe add ... -->
    <h1 class="title">Search Page</h1> 

    <!-- <form class="form-inline mb-10" @submit="Search"> -->
    <form class="form-inline mb-10" @submit.prevent="Search">
      <label for="number_of_res">Amount of results: </label>
      <div class="form-check form-check-inline" id="number_of_res">
        <input class="form-check-input" type="radio" name="inlineRadioOptions" v-model="formData.AmountOfResults" value="1">
        <label class="form-check-label" for="inlineRadio1">1</label>
      </div>
      <div class="form-check form-check-inline">
        <input class="form-check-input" type="radio" name="inlineRadioOptions" v-model="formData.AmountOfResults" value="2">
        <label class="form-check-label" for="inlineRadio2">2</label>
      </div>
      <div class="form-check form-check-inline">
        <input class="form-check-input" type="radio" name="inlineRadioOptions" v-model="formData.AmountOfResults" value="6">
        <label class="form-check-label" for="inlineRadio3">6</label>
      </div>
      <select v-model="formData.cuisine">
        <option v-for="option in options.cuisine" :key="option" :value="option">{{ option }}</option>
      </select>
      <select v-model="formData.diet">
        <option v-for="option in options.diet" :key="option" :value="option">{{ option }}</option>
      </select>
      <select v-model="formData.intolerance">
        <option v-for="option in options.intolerance" :key="option" :value="option">{{ option }}</option>
      </select>
      <!-- select from list... -->



      <input class="form-control" type="search" placeholder="Query to search" aria-label="Search" v-model="formData.query">
      <button class="btn" type="submit">Search</button>
    </form>
    <b-row v-for="(recipeLine, index) in recipeLines" :key="index" class="recipe-line">
      <b-col v-for="recipe in recipeLine" :key="recipe.id">
              <RecipePreview class="recipePreview" :recipe="recipe" />
      </b-col>
    </b-row>

  </div>
</template>

<script>
  // import RecipePreviewListSearch from "../components/RecipePreviewListSearch";
  import RecipePreview from '../components/RecipePreview.vue';
  export default {
    components: {
      RecipePreview,
      // RecipePreviewListSearch,
    },
    name: 'SearchPage',
    data() {
      return {
        formData: {
          query: '',
          number: 5,
          cuisine: undefined,
          diet: undefined,
          intolerance: undefined,
          fillIngredients: false,
          addRecipeInformation: false,
          AmountOfResults: 2
        },
        recipes: [],
        options:{
          cuisine: ["African", "American", "British", "Cajun", "Caribbean", "Chinese", "Eastern European", "European", "French", "German", "Greek", "Indian", "Irish", "Italian", "Japanese", "Jewish", "Korean", "Latin American", "Mediterranean", "Mexican", "Middle Eastern", "Nordic", "Southern", "Spanish", "Thai", "Vietnamese"],
          diet: ["Gluten Free", "Ketogenic", "Vegetarian", "Lacto-Vegetarian", "Ovo-Vegetarian", "Vegan", "Pescetarian", "Paleo", "Primal", "Whole30"],
          intolerance: ["Dairy", "Egg", "Gluten", "Grain", "Peanut", "Seafood", "Sesame", "Shellfish", "Soy", "Sulfite", "Tree Nut", "Wheat"]

        }
      }
    },
    computed: {
    recipeLines() {
      const recipesPerLine = 3;
      const lines = [];
      let currentLine = [];

      // Iterate over the recipes and split them into lines
      this.recipes.forEach((recipe, index) => {
        currentLine.push(recipe);

        // Check if the current line is full or if it's the last recipe
        if (currentLine.length === recipesPerLine || index === this.recipes.length - 1) {
          lines.push(currentLine);
          currentLine = [];
        }
      });
      console.log("recipeLines");
      console.log(lines);
      return lines;
    }
  },
    methods: {
      async Search() {
        try {
          this.recipes = [];
          const response = await this.axios.post(
          this.$root.store.server_domain + "/recipes/search",
          {
            query: this.formData.query,
            number: this.formData.AmountOfResults,
            cuisine: this.formData.cuisine,
            diet: this.formData.diet,
            intolerance: this.formData.intolerance,
            fillIngredients: this.formData.fillIngredients,
            addRecipeInformation: this.formData.addRecipeInformation
          },
          { withCredentials: true }
        );
        console.log("Search Page response");
        console.log(response);
        this.recipes.push(...response.data);

        console.log("Search Page recipes");
        console.log(this.recipes);
        // RecipePreviewListSearch.methods.updateRecipes2(recipes2);

        } catch (e) {
          console.log(e);
        }
      },

    }
  }
</script>

<style>

</style>