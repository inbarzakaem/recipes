<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <div v-for="r in recipes" :key="r.id">
      <RecipePreview  :recipe="r"></RecipePreview>
      <CheckFavAndSeen :recipe_id="r.id"></CheckFavAndSeen>
    </div>
    <b-button id="btnRandomRecipes" variant="primary" @click="updateRecipes">Change recipe</b-button> 
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
import CheckFavAndSeen from "./CheckFavAndSeen.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview: RecipePreview,
    CheckFavAndSeen: CheckFavAndSeen,
  },
  props: {
    title: {
      type: String,
      required: true
    },
  },
  data() {
    return {
      recipes: [
    {
        "id": 638808,
        "title": "Chocolate Almond Bark",
        "readyInMinutes": 45,
        "aggregateLikes": 34,
        "vegetarian": false,
        "vegan": false,
        "glutenFree": true,
        "image": "https://spoonacular.com/recipeImages/638808-556x370.jpg"
    },
    {
        "id": 665779,
        "title": "Zucchini Ribbon and Ricotta Pizza",
        "readyInMinutes": 45,
        "aggregateLikes": 6,
        "vegetarian": false,
        "vegan": false,
        "glutenFree": false,
        "image": "https://spoonacular.com/recipeImages/665779-556x370.jpg"
    },
    {
        "id": 637624,
        "title": "Cheesecake Ice-Cream With Mango Syrup",
        "readyInMinutes": 45,
        "aggregateLikes": 18,
        "vegetarian": true,
        "vegan": false,
        "glutenFree": true,
        "image": "https://spoonacular.com/recipeImages/637624-556x370.jpg"
    }  
      ]
    };
  }
//   mounted() {
//     this.updateRecipes();
//   },
//   data(){
//     return{
//       recipes: [],
//     }
//   },
//   methods: {
//     async updateRecipes() {
//       try {
//         const response = await this.axios.get(
//           this.$store.server_domain + "/recipes/random",
//         );

//         const recipes = response.data;
//         this.recipes = [];
//         this.recipes.push(...recipes);
//         // console.log(this.recipes);
//       } catch (error) {
//         console.log(error);
//       }
//     },
//   }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}

#btnRandomRecipes{
  margin-top: 2vh;
  margin-bottom: 5vh;
}
</style>
