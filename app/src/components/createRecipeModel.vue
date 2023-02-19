<template>
    <div>
     <!-- need to complete validation and fix instruction and ingredients to be multiple input that controledby user -->
    <b-modal id="recipeCreation" title="Create new recipe" @show="resetModal" @hidden="resetModal" hide-footer>
      <b-form-group label="Enter recipe title" label-for="recipeTitle" >
        <b-form-input id="recipeTitle" v-model="recipe.title" :state="recipeTitleState" placeholder="Title"></b-form-input>
        <b-form-invalid-feedback>
            Enter at least 1 character
        </b-form-invalid-feedback>
      </b-form-group>
      <b-form-group label="Add image URL" label-for="recipeImage">
        <b-form-input id="recipeImage" v-model="recipe.image" :state="recipeImageState" placeholder="Image's url"></b-form-input>
        <b-form-invalid-feedback>
            Enter image url
        </b-form-invalid-feedback>
      </b-form-group>
      <b-form-group label="Enter recipe's time preperation" label-for="recipeTimePreperation" placeholder="Time preperation">
        <b-form-input id="recipeTimePreperation" v-model="recipe.timePreperation" :state="recipeTimePreperationState"></b-form-input>
        <b-form-invalid-feedback>
            Enter at least 1 number
        </b-form-invalid-feedback>
      </b-form-group>
      <b-form-group label="Enter recipe's serving amount" label-for="recipeServing" placeholder="Serving">
        <b-form-input id="recipeServing" v-model="recipe.serving" :state="recipeServingState"></b-form-input>
        <b-form-invalid-feedback>
            Enter at least 1 digit
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group>
        <div v-for="(instructionStep, i) in recipe.instructions" :key="i">
          Add instruction:
          <b-form-input v-model="instructionStep.instruction" placeholder="Instruction" :state="recipeInstructionsState(instructionStep.instruction)"></b-form-input>
          <b-form-invalid-feedback>
            Enter at least 1 character
          </b-form-invalid-feedback>
          <br>
        </div>
        <b-button type="button" @click="addNewInstruction">Add instruction</b-button>
      </b-form-group>

      <b-form-group >
        <div v-for="(ingredientStep, i) in recipe.ingredients" :key="i">
          Add ingredient's details:
          <b-form-input v-model="ingredientStep.name" placeholder="ingredient's name" :state="recipeIngredientState(ingredientStep.name)"></b-form-input>
          <b-form-invalid-feedback>
            Enter at least 1 character
          </b-form-invalid-feedback>
          <br>
          <b-form-input v-model="ingredientStep.amount" placeholder="amount" :state="recipeIngredientAmountState(ingredientStep.amount)"></b-form-input>
          <b-form-invalid-feedback>
            Enter at least 1 character
          </b-form-invalid-feedback>
          <br>
        </div>
        <b-button type="button" @click="addNewIngredient">Add Ingredient</b-button>
      </b-form-group>
      <b-form-checkbox-group v-model="selected" :options="options" value-field="item" text-field="name">
      </b-form-checkbox-group>
      <br>
      <b-form-group >
        <b-button id="btnSubmit" variant="primary" @click="handleSubmit">Submit</b-button>
        <b-button id="btnCancel" variant="danger" @click="$bvModal.hide('recipeCreation')">Cancel</b-button>
      </b-form-group>
    </b-modal>
    </div>
</template>

<script>
export default {
  name: "createRecipeModel",
  data(){
    return{
      recipe:{
        title:'',
        image: '',
        timePreperation: '',
        ingredients: [{
          name:'',
          amount:'',
        }],
        instructions: [{
          instruction:'',
        }],
        serving: '',
        vegan: false,
        vegetarian: false,
        glutenFree: false,
        aggregateLikes:0,
      },
      selected:[],
      options:[
        {item: 'Vegan', name: 'Vegan'},
        {item: 'Vegetarian', name: 'Vegetarian'},
        {item: 'Gluten', name: 'Gluten'}
        ],
    }
  },
  computed:{
    recipeTitleState(){
        return this.recipe.title.length === 0 ? false : true;
    },
    recipeImageState(){
        return this.recipe.image.length === 0 ? false : true;
    },
    recipeTimePreperationState(){
        return isNaN(parseInt(this.recipe.timePreperation, 10)) ? false : true;
    },
    recipeServingState(){
        return this.recipe.serving.length === 0 ? false : true;
    },
  },
  methods: {
    recipeInstructionsState(instruction){
      return instruction.length === 0 ? false : true;
    },
    recipeIngredientState(name){
      return name.length === 0 ? false : true;
    },
    recipeIngredientAmountState(amount){
      return amount.length === 0 ? false : true;
    },
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
    resetModal(){
      this.recipe.title = '';
      this.selected = [];
      this.recipe.image = '';
      this.recipe.timePreperation = '';
      this.recipe.ingredients= [{
        name:'',
        amount:'',
      }];
      this.recipe.instructions= [{
        instruction:'',
      }];
      this.recipe.serving= '';
      this.recipe.vegan = false;
      this.recipe.vegetarian = false;
      this.recipe.glutenFree = false;
    },
    // 1. need to delete the session check in the end because we already shold have cookies.
    // 2. need to validate that we dont send empty field to server.
     async handleSubmit(){
      if(this.recipe.title && this.recipe.image && this.recipe.timePreperation && this.recipe.ingredients[0].name && 
           this.recipe.ingredients[0].amount && this.recipe.instructions[0].instruction && this.recipe.serving){
        const session = this.$cookies.get("session");
        if(this.selected.includes('Vegan')){
          this.recipe.vegan = true;
        }
        if(this.selected.includes('Vegetarian')){
          this.recipe.vegetarian = true;
        }
        if(this.selected.includes('Gluten')){
          this.recipe.glutenFree = true;
        }
        this.recipe.timePreperation = parseInt(this.recipe.timePreperation);
        this.recipe.serving = parseInt(this.recipe.serving);
        if(session){
              const response = await this.axios.post(this.$store.server_domain + "/users/my_recipes",
              {
                recipe: this.recipe,
              },
              {
                withCredentials: true,
              }
        )}
        else{
          alert("no cookies, need to re-login");
        }
        // close model
        this.$bvModal.hide('recipeCreation');
      }
    },
    addNewInstruction(){
      this.recipe.instructions.push({
        instruction: '',
      });

    },
    addNewIngredient(){
      this.recipe.ingredients.push({
        name:'',
        amount:'',
      })
    }
  }
};
</script>
<style scoped>
  #btnSubmit{
    margin-left: 15vh;


  }
  #btnCancel{
   margin-left: 5vh;
  }
</style>