<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <div v-for="r in recipes" :key="r.id">
      <RecipePreview class="recipePreview" :recipe="r" />
      <CheckFavAndSeenVue :recipe_id="r.id"></CheckFavAndSeenVue>
    </div>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
import CheckFavAndSeenVue from "./CheckFavAndSeen.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview,
    CheckFavAndSeenVue,
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
          this.$store.server_domain + "/users/recipes",
          {
            withCredentials: true
          }
        );
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
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
</style>
