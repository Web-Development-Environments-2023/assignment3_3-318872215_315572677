<template>
  <div class="container">
    <!-- maybe add ... -->
    <h1 class="title">Search Page</h1> 
    <form class="form-inline mb-10" @submit="Search">
    <!-- <form class="form-inline mb-10" @submit.prevent="Search"> -->

      <!-- select from list... -->



      <input class="form-control" type="search" placeholder="Query to search" aria-label="Search" v-model="formData.query">
      <button class="btn" type="submit">Search</button>
    </form>
    <RecipePreviewListSearch v-if="this.recipes !== []" title="search results" :recipesArray="recipes" /> 

  </div>
</template>

<script>
  import RecipePreviewListSearch from "../components/RecipePreviewListSearch";
  export default {
    components: {
      RecipePreviewListSearch,
    },
    name: 'SearchPage',
    data() {
      return {
        formData: {
          query: '',
          number: 5,
          cuisine: "Italian",
          diet: "Vegetarian",
          intolerance: "Seafood",
          fillIngredients: true,
          addRecipeInformation: true
        },
        recipes: []
      }
    },
    methods: {
      async Search() {
        try {
          console.log("t1");
          console.log(this.formData.query);

          const response = await this.axios.post(
          this.$root.store.server_domain + "/recipes/search",
          {
            query: this.formData.query,
            number: 5,
            cuisine: "Italian",
            diet: "Vegetarian",
            intolerance: "Seafood",
            fillIngredients: true,
            addRecipeInformation: true
          }
        );
        console.log(response);
        this.recipes = [];
        const recipes = response.data;
        console.log(recipes);
        console.log("t2");
        RecipePreviewListSearch.methods.updateRecipes(recipes);

        } catch (e) {
          console.log(e);
        }
      },

    }
  }
</script>

<style>

</style>