<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1 class="text-center">{{ recipe.title }}</h1>
        <img :src="recipe.image" class="img-fluid rounded mx-auto d-block" alt="Recipe Image" />
      </div>
      <div class="recipe-body">
        <div class="row">
          <div class="col-md-6">
            <div class="mb-4">
              <h3>Recipe Details</h3>
              <hr />
              <template v-if="recipe.vegetarian">
                <img src="@/assets/vegetarian.png" width="60" height="60" id="icon" />
              </template>
              <template v-if="recipe.glutenFree">
                <img src="@/assets/glutenFree.jpg" width="60" height="60" id="icon" />
              </template>
              <template v-if="recipe.vegan">
                <img src="@/assets/vegan.jpg" width="60" height="60" id="icon" />
              </template>
   
              <div >
                <div class="me-4">
                  <strong>Ready in:</strong>
                  <span class="text-muted">{{ recipe.readyInMinutes }} minutes </span>
                </div>
                <div>
                  <strong>Likes:</strong>
                  <span class="text-muted">{{ recipe.popularity }} likes </span>
                </div>
                <div>
                  <strong>Servings :</strong>
                  <span class="text-muted">{{ recipe.servings }} servings</span>
                </div>
              </div>
            </div>
            <div class="mb-4">
              <h3>Ingredients</h3>
              <hr />
              <ul>
                <li v-for="(r, index) in recipe.ingredients" :key="index + '_' + r.id">{{ r.original }}</li>
              </ul>
            </div>
          </div>
          <div class="col-md-6">
            <div class="mb-4">
              <h3>Instructions</h3>
              <hr />
              <ol>
                <div v-html="formattedInstructions"></div>
              </ol>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
export default {
  data() {
    return {
      recipe: null
    };
  },
  computed: {
    formattedInstructions() {
      return this.recipe.instructions;
    }
  },
  async created() {
    console.log("@@@@@@RecipeViewPage@@@@@@@@@@@@@@@@@@@@");
      console.log(this.$route.params.recipeId);
    try {
      let response;
      // response = this.$route.params.response;

      try {
        response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/recipes/info",
          this.$root.store.server_domain + "/recipes/"+this.$route.params.recipeId,
          { withCredentials: true }
          // this.$root.store.server_domain + "/recipes/info",
          // {
          //   params: { id: this.$route.params.recipeId }
          // }
        );

        // console.log("response.status", response.status);
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }


      let {
        id,
        title,
        readyInMinutes,
        image,
        popularity,
        vegan,
        vegetarian,
        glutenFree,
        instructions,
        servings,
        ingredients
      } = response.data;
      // let {
      //   analyzedInstructions,
      //   instructions,
      //   extendedIngredients,
      //   aggregateLikes,
      //   readyInMinutes,
      //   image,
      //   title
      // } = response.data.recipe;

      // let _instructions = analyzedInstructions
      //   .map((fstep) => {
      //     fstep.steps[0].step = fstep.name + fstep.steps[0].step;
      //     return fstep.steps;
      //   })
      //   .reduce((a, b) => [...a, ...b], []);

      let _recipe = {
        id,
        title,
        readyInMinutes,
        image,
        popularity,
        vegan,
        vegetarian,
        glutenFree,
        instructions,
        servings,
        ingredients
      };
      // let _recipe = {
      //   instructions,
      //   _instructions,
      //   analyzedInstructions,
      //   extendedIngredients,
      //   aggregateLikes,
      //   readyInMinutes,
      //   image,
      //   title
      // };

      this.recipe = _recipe;
      console.log("this.recipe", this.recipe);

    } catch (error) {
      console.log(error);

    }
  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
