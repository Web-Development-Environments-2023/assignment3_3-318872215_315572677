<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-row>
      <b-col v-for="r in recipesArray" :key="r.id">
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

      // console.log(recipesArray1);
      // console.log("test3");

      // if (recipesArray !== undefined) {
      //   console.log(recipesArray);
      // }

      // this.recipes.push(...recipes);
      // try {
    },
    async updateRecipes2(recipesArray1) {
      // console.log("test1");
      // console.log(this.title);
      // console.log("test2");

      // console.log(recipesArray1);
      // console.log("test3");
      // this.recipesArray = recipesArray1;
      // console.log(this.recipesArray);
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