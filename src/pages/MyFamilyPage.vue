<template>
    <div>
        <div id="background"></div>

        <h1>My Family</h1>
        <p>My Family Page</p>
        <b-col v-for="recipe in recipes" :key="recipe.id">
              <br>
              <MyRecipePreview class="recipePreview" :recipe="recipe" />
        </b-col>
    </div>
</template>

<script>
import MyRecipePreview from '../components/MyRecipePreview.vue';

export default {
    name: 'MyFamilyPage',
    components: {
        MyRecipePreview,        
    },
    data() {
        return {
            recipes: [],
        }
    },
    async created() {
        console.log("Search Page created ");
        try {
          this.recipes = [];
          const response = await this.axios.get(
          this.$root.store.server_domain + "/users/familyRecipes",
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
     mounted() {
 
     },
    methods: {
        
    },
    computed: {
        
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