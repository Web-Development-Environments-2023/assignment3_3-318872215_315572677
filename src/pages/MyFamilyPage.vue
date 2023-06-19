<template>
    <div>
        <div id="background"></div>
        <h1>My Family</h1>
        <p>My Family Page</p>
        <InternalRecipesGridCards :recipesArray="this.recipes" />
    </div>
</template>

<script>
import InternalRecipesGridCards from '../components/InternalRecipesGridCards.vue';
export default {
    name: 'MyFamilyPage',
    components: {
      InternalRecipesGridCards,        
    },
    data() {
        return {
            recipes: [],
        }
    }, 
    async created() {
      try {
        this.recipes = [];
        const response = await this.axios.get(
        this.$root.store.server_domain + "/users/familyRecipes",
        { withCredentials: true }
      );
      console.log("MyFamilyPage response");
      console.log(response);
      this.recipes.push(...response.data);

      console.log("MyFamilyPage recipes");
      console.log(this.recipes);
      } catch (e) {
        console.log(e);
      } 
    },
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