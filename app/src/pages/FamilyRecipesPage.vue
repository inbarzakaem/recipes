<template>
  <div class="container">
    <h1>My Family Recipes</h1>
    
    <div v-for="(recipe, index) in familyRecipes" :key="index">
        <div class="recipe-body" @mouseover="hover=true" @mouseleave="hover=false">
      <img :src="recipe.image" class="recipe-image" />
      <span v-if="hover">click on image</span>
    </div>
    <div class="recipe-footer">
      <div :owner="recipe.owner" class="recipe-owner">
        {{ recipe.owner }}
      </div>
      <div :title="recipe.title" class="recipe-title">
        {{ recipe.title }}
      </div>
      <div :id="recipe.id" class="recipe-id">
        {{ recipe.id }}
      </div>
      <ul class="recipe-overview">
        <strong>Recipe Time: </strong>
        <li>{{ recipe.recipeTime}} </li>
        
        <strong>Ingredients: </strong>
        <li v-for="(value, key,index) in recipe.ingrediants"  :key="index">
        {{ value }}
        </li>

        <strong>Instructions: </strong>
        <li v-for="(value, key,index) in recipe.instructions"  :key="index">
         {{ value }}
        </li>

        <!-- <li>{{ recipe.aggregateLikes }} likes</li>
        <li v-if="recipe.recipeTime">vegan</li>
        <li v-if="recipe.vegetarian">vegetarian</li>
        <li v-if="recipe.glutenFree">gluten free</li> -->
      </ul>
    </div>
    </div>
  </div>
</template>

<script>

export default {
  name: "FamilyRecipesPage",
  data() {
    return {
      familyRecipes: []
    };
  },
  methods: {
    async getFamilyRecipes() {
      try {
        // this.axios.defaults.withCredentials = true;
        const response = await this.axios.get(
          this.$store.server_domain +"/users/family_recipe",  
          {
            withCredentials: true

          }
        );
        console.log(response);
        const recipes = response.data;
        console.log(recipes);
        this.familyRecipes.push(...recipes);
      } catch (err) {
        console.log(err.response);
      }
    }
  },
  mounted() {
    this.getFamilyRecipes();
  }
};
</script>

<style lang="scss" scoped>
.container {
  max-width: 520px;
}
.recipe-image{
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}

</style>
