<template>
  <div>
    <h1 id="pageTitle">My Favorite Recipes</h1>
    <div class="container" v-for="(recipe, i) in favoriteRecipes" :key="i">
      <RecipePreviewVue :recipe="recipe"></RecipePreviewVue>
      <CheckFavAndSeenVue :recipe_id="recipe.id"></CheckFavAndSeenVue>
    </div>
  </div>
</template>

<script>
import RecipePreviewVue from '../components/RecipePreview.vue';
import CheckFavAndSeenVue from '../components/CheckFavAndSeen.vue';
export default {
  name: "MyFavoritesPage",
  components: {
    RecipePreviewVue: RecipePreviewVue,
    CheckFavAndSeenVue: CheckFavAndSeenVue,
  },
    mounted() {
    this.getMyFavoriteRecipes();
  },
  data() {
    return {
      favoriteRecipes: []
    };
  },
  methods: {
    async getMyFavoriteRecipes() {
      try {
        const response = await this.axios.get(
          this.$store.server_domain + "/users/favorites",
          {
            withCredentials: true,
          }
        );
        const recipes = response.data;
        this.favoriteRecipes.push(...recipes);
      } catch (err) {
        console.log(err.response);
      }
    }
  },
};
</script>

<style lang="scss" scoped>
.container {
  max-width: 550px;
}

#pageTitle{
  text-align: center;
  margin-bottom: 5vh;
}
</style>

