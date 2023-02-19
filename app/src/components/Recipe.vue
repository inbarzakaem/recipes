<template>
<div>
    
    <RecipePreview :recipe="recipe.recipePreview"></RecipePreview>
    {{RecipePreview}}

    <div class="div">
        <strong> Servings:</strong>
        <li v-on="recipe.servings"> {{recipe.servings}}</li>
        
    </div>
    <div>
        <strong>Instructions:</strong>
        <div v-if="recipe.instructions[0] === '<'"  v-html="recipe.instructions"></div>
        <div v-else>
            <ol class="div" >
                <li v-for="(instructions, index) in splitInstructions()" :key="index">{{ instructions }}</li>
            </ol>
        </div>


        </div>
        <strong>Ingredients: </strong>
        <ul>
        <li v-for="(value, key,index) in recipe.ingredients"  :key="index">
         {{ key }}: {{ value }}
         </li>
        </ul>
        

    </div>

</template>
<script>
import RecipePreview from "../components/RecipePreview";
export default {
    components: {
        RecipePreview: RecipePreview
    },
    props: {
            recipe: {
            type: Object,
            required: true
        }
    },
    methods:{
        // async inFavorited(){
        //     const response = await this.axios.get(
        //           this.$store.server_domain + "/users/favorites/",
        //           {
        //             withCredentials: true
        //           });
        // },
        splitInstructions(){
            return this.recipe.instructions.split('.').filter(word => word.trim().length > 0);
        }

    }
}
    
</script>
<style scoped>

</style>