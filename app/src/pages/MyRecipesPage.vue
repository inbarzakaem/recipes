<template>
  <div class="container">
    <h1 class="title">My Recipes Page</h1>
    <div v-for="(r, index) in recipes" :key="index">
      <RecipePreviewVue :recipe="r"></RecipePreviewVue>
    </div>
  </div>
</template>

<script>
import RecipePreviewVue from '../components/RecipePreview.vue';
export default {
  components:{
    RecipePreviewVue: RecipePreviewVue,
  },
  data(){
    return{
      recipes: [],
    }
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          this.$store.server_domain + "/users/my_recipes",
          {
            withCredentials: true,
          }
        );
        const recipesFromServer = response.data;
        this.recipes.push(...recipesFromServer);
        
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
  }

</script>

<style scoped>


</style> 
