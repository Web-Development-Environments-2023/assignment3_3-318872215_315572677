<template>
  <div class="container">
    <div id="background"></div>
    
    <h1 class="title">Search Page</h1>

    <form class="form-inline mb-10" @submit.prevent="Search">

      <div class="form-group">
        <label for="number_of_res">Amount of results:</label>
        <select class="form-control" v-model="formData.AmountOfResults" id="Amount">
          <option v-for="option in options.AmountOfResults" :key="option" :value="option">{{ option }}</option>
        </select>
      </div>

      <div class="form-group">
        <label for="cuisine">Cuisine:</label>
        <select class="form-control" v-model="formData.cuisine" id="cuisine">
          <option v-for="option in options.cuisine" :key="option" :value="option">{{ option }}</option>
        </select>
      </div>

      <div class="form-group">
        <label for="diet">Diet:</label>
        <select class="form-control" v-model="formData.diet" id="diet">
          <option v-for="option in options.diet" :key="option" :value="option">{{ option }}</option>
        </select>
      </div>

      <div class="form-group">
        <label for="intolerance">Intolerance:</label>
        <select class="form-control" v-model="formData.intolerance" id="intolerance">
          <option v-for="option in options.intolerance" :key="option" :value="option">{{ option }}</option>
        </select>
      </div>

      <div class="form-group">
        <label for="query">Query to search:</label>
        <input class="form-control" type="search" placeholder="Enter search query" aria-label="Search" v-model="formData.query" id="query">
      </div>

      <button class="btn btn-primary" type="submit">Search</button>
    </form>

    <b-row v-for="(recipeLine, index) in recipeLines" :key="index" class="recipe-line">
      <b-col v-for="recipe in recipeLine" :key="recipe.id">
        <br>
              <RecipePreview class="recipePreview" :recipe="recipe" />
      </b-col>
    </b-row>

    <p>Search URL: {{ searchUrl }}</p>

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
          intolerance: ["Dairy", "Egg", "Gluten", "Grain", "Peanut", "Seafood", "Sesame", "Shellfish", "Soy", "Sulfite", "Tree Nut", "Wheat"],
          AmountOfResults: [1, 2, 3, 6]
        },
        searchUrl: null,
      }
    },

    mounted() {
      const storedValue = localStorage.getItem('search_url');
      if (storedValue) {
        this.searchUrl = JSON.parse(storedValue);
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
        // console.log("Search Page response");
        // console.log(response);
        this.recipes.push(...response.data);

        console.log("##################################################################");
        console.log("Search Page recipes");
        console.log(this.formData);
        this.$root.store.last_search(...this.formData);
        console.log("##################################################################");


        } catch (e) {
          console.log(e);
        }
      },

    }
  }
</script>

<style>
#background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url(../assets/food-background.jpg);
  background-size: cover;
  background-position: center;
  z-index: -1;
  opacity: 0.5;
}
</style>