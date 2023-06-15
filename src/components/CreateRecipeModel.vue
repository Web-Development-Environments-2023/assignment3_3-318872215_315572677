<template>
    <div class="container">
        <!-- left side model -->
        <div class="left">
            <b-form @submit.prevent="createRecipeForm" @reset.prevent="resetForm">
            
                <!-- Title Recipe -->
                <b-form-group id="input-group-title" label-cols-sm="3" label="Recipe Name:" label-for="title">
                    <b-form-input id="title" v-model="$v.form.title.$model" type="text" :state="validateState('title')"></b-form-input>
                    <b-form-invalid-feedback v-if="!$v.form.title.required"> Name is required </b-form-invalid-feedback>
                </b-form-group>

                <!-- Preperation Time -->
                <b-form-group id="input-group-readyInMinutes" label-cols-sm="3" label="Preperation Time [Min]:" label-for="readyInMinutes">
                    <b-form-input id="readyInMinutes" v-model="$v.form.readyInMinutes.$model" type="text" :state="validateState('readyInMinutes')"></b-form-input>
                    <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.required"> Preperation time is required </b-form-invalid-feedback>
                    <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.integer"> Preperation time must be number </b-form-invalid-feedback>
                </b-form-group>

                <!-- Serving Amount -->
                <b-form-group id="input-group-servings" label-cols-sm="3" label="Serving amount:" label-for="servings">
                    <b-form-input id="servings" v-model="$v.form.servings.$model" type="text" :state="validateState('servings')"></b-form-input>
                    <b-form-invalid-feedback v-if="!$v.form.servings.required"> Serving amount is required </b-form-invalid-feedback>
                    <b-form-invalid-feedback v-if="!$v.form.servings.integer"> Serving amount must be number </b-form-invalid-feedback>
                </b-form-group>

                <!-- Gluten -->
                <b-form-group id="input-group-glutenFree" label-cols-sm="3" label="Gluten free:" label-for="glutenFree">
                    <b-form-select id="glutenFree" v-model="$v.form.glutenFree.$model" :options="[false, true]" :state="validateState('glutenFree')"></b-form-select>
                    <small v-if="$v.form.glutenFree.$model">please make sure your recipe is indeed gluten free</small>
                </b-form-group>

                <!-- Vegeterian -->
                <b-form-group id="input-group-vegeterian" label-cols-sm="3" label="Vegeterian:" label-for="vegeterian">
                    <b-form-select id="vegeterian" v-model="$v.form.vegeterian.$model" :options="[false, true]" :state="validateState('vegeterian')"></b-form-select>
                    <small v-if="$v.form.vegeterian.$model">please make sure your recipe is indeed vegeterian</small>
                </b-form-group>

                <!-- Vegan -->
                <b-form-group id="input-group-vegan" label-cols-sm="3" label="Vegan:" label-for="vegan">
                    <b-form-select id="vegan" v-model="$v.form.vegan.$model" :options="[false, true]" :state="validateState('vegan')"></b-form-select>
                    <small v-if="$v.form.vegan.$model">please make sure your recipe is indeed vegan</small>
                </b-form-group>

                <br>

                


            </b-form>
        </div>


        <!-- right side model -->
        <div class="right">
 
        </div>

    </div>
</template>

<script>
import { required, integer } from "vuelidate/lib/validators";
export default {
    data() {
        return {
        form: {
            title: "",
            readyInMinutes: "",
            servings: "",
            glutenFree: false,
            vegeterian: false,
            vegan: false,



            submitError: undefined,
            },
        submited: false,
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
            vegeterian: {},
            vegan: {},
            
        }
    },
    methods: {
        validateState(param) {
            const { $dirty, $error } = this.$v.form[param];
            return $dirty ? !$error : null;
        },

        async createRecipeForm() {
            this.pressed = true;
        },

        resetForm(){
            this.form = {
                title: "",
                readyInMinutes: "",
                servings: "",
                glutenFree: false,
                vegeterian: false,
                vegan: false,
            }
        }
    }
}
</script>


<style>
.right{
    float: right;
}

.left{  
    float: left;
}
</style>