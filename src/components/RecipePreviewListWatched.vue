<!-- <template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <table class="recipe-table">
      <tbody>
        <tr v-for="r in recipes" :key="r.id">
          <td class="recipe-name">
            <RecipePreview class="recipePreview" :recipe="r" />
          </td>
        </tr>
      </tbody>
    </table>
  </b-container>
</template> -->

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
  </b-container>
</template>
  
  <script>
  import RecipePreview from "./RecipePreview.vue";
  export default {
    name: "RecipePreviewList",
    components: {
      RecipePreview
    },
    props: {
      title: {
        type: String,
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
      async updateRecipes() {
        try {
          const response = await this.axios.get(
            this.$root.store.server_domain + "/users/lastWatched",
            { withCredentials: true }
          );

        // sessionStorage.setItem(
        //   "watchedRecipes",
        //   JSON.stringify(response.data)
        // );
  
          // console.log(response);
          this.recipes = [];
          const recipes = response.data;
          console.log(recipes);
          this.recipes.push(...recipes);
          // console.log(this.recipes);
        } catch (error) {
          console.log(error);
        }
      }
    }
  };
  </script>
  

  <style lang="scss" scoped>
  .container {
    min-height: 400px;
  }
  
  .recipe-table {
    width: 100%;
    border-collapse: collapse;
    table-layout: fixed;
  }
  
  .recipe-table th,
  .recipe-table td {
    padding: 10px;
    text-align: left;
  }
  
  .recipe-table th {
    background-color: #f2f2f2;
  }
  
  .recipe-table tr:nth-child(even) {
    background-color: #f9f9f9;
  }
  
  .recipe-name {
    width: 40%;
  }
  </style>
  
  