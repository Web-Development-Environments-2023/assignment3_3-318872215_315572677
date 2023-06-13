<template>
  <div class="container">
    <h1 class="title">Search Page</h1>
    <form class="form-inline mb-10" @submit.prevent="Search">
      <input class="form-control" type="search" placeholder="Query to search" aria-label="Search"
             v-model="form.query">
      <button class="btn" type="submit">Search</button>
    </form>
    <RecipePreviewListSearch title="search results" query="" class="RecipePreviewListSearch center"  /> 

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
        form: {
          query: ''
        }
      }
    },
    methods: {
      async Search() {
        try {
          console.log("123534erg");

          const response = await this.axios.post(
          this.$root.store.server_domain + "/recipes/search",
          {
            query: this.form.query,
            number: 5,
            cuisine: "Italian",
            diet: "Vegetarian",
            intolerance: "Seafood",
            fillIngredients: true,
            addRecipeInformation: true
          }
        );
        console.log(response.data);
        } catch (e) {
          console.log(e);
        }
      }
    }
  }
</script>

<style>

</style>
