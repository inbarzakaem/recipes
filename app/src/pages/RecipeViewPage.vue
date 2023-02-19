<template>
 <div>
      <Recipe class="center" :recipe="recipeData" />
      <CheckFavAndSeenVue class="center" :recipe_id="recipeData.recipePreview.id"></CheckFavAndSeenVue>
 </div>
</template>

<script>
import Recipe from "../components/Recipe";
import CheckFavAndSeenVue from "../components/CheckFavAndSeen.vue";
export default {
  components: {
    Recipe: Recipe,
    CheckFavAndSeenVue: CheckFavAndSeenVue,
  },
  data() {
    return {
      recipeData: {
        type: Object,
        require: true
      }
    };
  },
  async mounted() {
    try {
      let response;
      try {
        response = await this.axios.get(
          this.$store.server_domain + "/recipes/" + this.$route.params.recipeId + "/details",
          {
            withCredentials: true
          }
        );
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
      let {
        instructions,
        servings,
        ingredients,
        recipePreview,
      } = response.data;
      this.recipeData = response.data;
    } catch (error) {
      console.log(error);
    }
  }

};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>