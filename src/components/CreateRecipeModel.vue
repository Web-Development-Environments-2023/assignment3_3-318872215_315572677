<template>
  <div class="container">
    <!-- left side model -->
    <div class="left">
      <b-form @submit.prevent="createRecipeForm" @reset.prevent="resetForm">
        <!-- Title Recipe -->
        <b-form-group id="input-group-title" label-cols-sm="3" label="Recipe Name:" label-for="title">
          <b-form-input id="title" v-model.trim="$v.form.title.$model" type="text" :state="validateState('title')"></b-form-input>
          <b-form-invalid-feedback v-if="!$v.form.title.required">Name is required</b-form-invalid-feedback>
        </b-form-group>

        <!-- Preperation Time -->
        <b-form-group id="input-group-readyInMinutes" label-cols-sm="3" label="Preparation Time [Min]:" label-for="readyInMinutes">
          <b-form-input id="readyInMinutes" v-model.trim="$v.form.readyInMinutes.$model" type="text" :state="validateState('readyInMinutes')"></b-form-input>
          <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.required">Preparation time is required</b-form-invalid-feedback>
          <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.integer">Preparation time must be a number</b-form-invalid-feedback>
        </b-form-group>

        <!-- Serving Amount -->
        <b-form-group id="input-group-servings" label-cols-sm="3" label="Serving amount:" label-for="servings">
          <b-form-input id="servings" v-model.trim="$v.form.servings.$model" type="text" :state="validateState('servings')"></b-form-input>
          <b-form-invalid-feedback v-if="!$v.form.servings.required">Serving amount is required</b-form-invalid-feedback>
          <b-form-invalid-feedback v-if="!$v.form.servings.integer">Serving amount must be a number</b-form-invalid-feedback>
        </b-form-group>

        <!-- Gluten -->
        <b-form-group id="input-group-glutenFree" label-cols-sm="3" label="Gluten free:" label-for="glutenFree">
          <b-form-select id="glutenFree" v-model.trim="$v.form.glutenFree.$model" :options="[false, true]" :state="validateState('glutenFree')"></b-form-select>
          <small v-if="$v.form.glutenFree.$model">Please make sure your recipe is indeed gluten-free</small>
        </b-form-group>

        <!-- Vegetarian -->
        <b-form-group id="input-group-vegetarian" label-cols-sm="3" label="Vegetarian:" label-for="vegetarian">
          <b-form-select id="vegetarian" v-model.trim="$v.form.vegetarian.$model" :options="[false, true]" :state="validateState('vegetarian')"></b-form-select>
          <small v-if="$v.form.vegetarian.$model">Please make sure your recipe is indeed vegetarian</small>
        </b-form-group>

        <!-- Vegan -->
        <b-form-group id="input-group-vegan" label-cols-sm="3" label="Vegan:" label-for="vegan">
          <b-form-select id="vegan" v-model.trim="$v.form.vegan.$model" :options="[false, true]" :state="validateState('vegan')"></b-form-select>
          <small v-if="$v.form.vegan.$model">Please make sure your recipe is indeed vegan</small>
        </b-form-group>

      <!-- Instructions -->
      <b-form-group id="input-group-instructions" label-cols-sm="3" label="Instructions:" label-for="instructions">
        <b-form-textarea id="instructions" v-model.trim="$v.form.instructions.$model" placeholder="Recipe Instructions" rows="3"></b-form-textarea>
      </b-form-group>

        <!-- Ingredients -->
        <b-form-group id="input-group-ingredients" label-cols-sm="3" label="Ingredients:" label-for="ingredients">
          <div style="display: inline-flex;">
            <b-form-input placeholder="ingredient name" id="ingredient-name" v-model.trim="ingredientName"></b-form-input>
            <b-form-input placeholder="ingredient amount" id="ingredient-amount" v-model.trim="ingredientAmount"></b-form-input>
            <b-button v-b-tooltip.hover.top="'Your ingredients successfully added to the list!'" class="mt-2" variant="primary" small @click="insertIngredient">➕ Ingredient</b-button>
          </div>
        </b-form-group>

        <b-button type="submit" variant="primary" style="width: 250px;" class="ml-5 w-75">➕ Recipe</b-button>
        <b-button type="reset" variant="danger" @click="resetForm">Reset</b-button>
      </b-form>
    </div>

    <!-- right side model -->
    <div class="right">
      <!-- ingredients list -->
      <div class="ingredients">
        <h5>Added ingredients:</h5>
        <ul v-if="$v.form.ingredients.$model !== ''">
          <li class="list-group-item borderless" v-for="(ingredient, index) in $v.form.ingredients.$model.split('&')" :key="index + '_' + ingredient">
            <div v-if="index !== ($v.form.ingredients.$model.split('&').length - 1)">
              {{ ingredient }}
              <b-button @click="removeIngredient(ingredient)" size="sm" class="ml-2" variant="danger">❌</b-button>
            </div>
          </li>
        </ul>
        <small v-else>You haven't added any ingredients yet</small>
      </div>

      <!-- Photo -->
      <div class="photo" style="display: inline-flex; align-items: center;">
        <h3 style="margin-right: 10px;">Photo <span style="font-size: 14px; color: #ff9900;">[hyperlink]</span></h3>
        <input v-model.trim="$v.form.image.$model" placeholder="https://www.exampleImage.com" style="border: 1px solid #ccc; border-radius: 4px; padding: 6px 10px;">
      </div>
    </div>
  </div>
</template>

<script>
import { required, integer } from "vuelidate/lib/validators";
//   import { Tooltip } from 'bootstrap-vue';
export default {
  // directives: {
  //   'b-tooltip': Tooltip,
  // },
  data() {
    return {
      form: {
        id: null,
        title: "",
        readyInMinutes: "",
        servings: "",
        glutenFree: false,
        vegetarian: false,
        vegan: false,
        instructions: "",
        ingredients: "",
        image: "",

        popularity: 0,
        isWatched: null,
        inFavorites: null,

        submitError: undefined,
      },
      ingredientName: "",
      ingredientAmount: "",
      submitted: false,
    };
  },
  validations: {
    form: {
      title: {
        required,
      },
      readyInMinutes: {
        required,
        integer,
      },
      servings: {
        required,
        integer,
      },
      glutenFree: {},
      vegetarian: {},
      vegan: {},
      instructions: {},
      ingredientName: {},
      ingredientAmount: {},
      ingredients: {},
      image: {},
    },
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    closeModel() {
      this.showModal = false;
      this.resetForm();
    },
    async createRecipeForm() {
      this.pressed = true;

      const response = await this.axios.post(
        this.$root.store.server_domain + "/recipes/create",
        {
          "title": this.form.title,
          "readyInMinutes": this.form.readyInMinutes,
          "vegetarian": this.form.vegetarian,
          "vegan": this.form.vegan,
          "glutenFree": this.form.glutenFree,
          "image": this.form.image,
          "servings": this.form.servings,
          "instructions": this.form.instructions,
          "ingredients": this.form.ingredients
        },
        { withCredentials: true },
      );

      this.closeModal();
    },
    resetForm() {
      this.form = {
        id: null,
        title: "",
        readyInMinutes: "",
        servings: "",
        glutenFree: false,
        vegetarian: false,
        vegan: false,
        instructions: "",
        ingredients: "",
        image: "",

        popularity: 0,
        isWatched: null,
        inFavorites: null,

        submitError: undefined,
      };
      this.ingredientName = "";
      this.ingredientAmount = "";
      this.submitted = false;
    },
    insertIngredient() {
      if (this.ingredientName === "" || this.ingredientAmount === "") return;
      this.form.ingredients += `${this.ingredientName} | ${this.ingredientAmount} & `;
      this.ingredientName = "";
      this.ingredientAmount = "";
    },
    removeIngredient(ingredient) {
      const ingredients = this.form.ingredients.split("&");
      const index = ingredients.indexOf(ingredient);
      if (index > -1) {
        ingredients.splice(index, 1);
        this.form.ingredients = ingredients.join("&").trim();
      }
    },
  },
};
</script>


<style scoped>
/* Add your custom CSS styles here */

.recipe-form {
display: flex;
justify-content: space-between;
align-items: flex-start;
margin: 20px;
}

.left {
width: 60%;
float: left;
}

.right {
width: 40%;
float: right;
}

.ingredients {
margin-top: 20px;
}

.ingredient {
display: flex;
justify-content: space-between;
align-items: center;
margin-bottom: 5px;
}

.photo {
margin-top: 20px;
}

/* Add more custom styles as needed */
</style>