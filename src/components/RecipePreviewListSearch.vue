<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
    <b-button class="getThreeRandom_btn" @click="updateRecipes" variant="secondary">More Recipe</b-button>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewListSearch",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    },
    recipesArray: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes(recipesArray1) {
      console.log("test1");
      console.log(this.title);
      console.log("test2");

      console.log(recipesArray1);
      console.log("test3");

      this.recipes = recipesArray1;

      console.log(this.recipes);
      this.recipes.push(...recipes);
      // try {

      //   console.log("test4");

      //   const response = await this.axios.get(
      //     this.$root.store.server_domain + "/recipes/random",
      //     // "https://test-for-3-2.herokuapp.com/recipes/random"
      //   );
      //   console.log("test5");
      //   console.log(response);
      //   this.recipes = [];
      //   const recipes = response.data;
      //   console.log(recipes);
      //   this.recipes.push(...recipes);
      //   console.log(this.recipes);
      // } catch (error) {
      //   console.log(error);
      // }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
.getThreeRandom_btn{
  margin-left: 170px;
  margin-top: 50px;
  margin-bottom: 50px;
  width: 200px;
  
}
</style>