<template>
  <div class="container">
    <h1 class="title">Main Page</h1>
    <b-row>
      <b-col>
        <RecipePreviewList title="Explore this recipes" class="RandomRecipes center" ></RecipePreviewList> 
      </b-col>
      <!-- <b-col>
        <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link>
        <RecipePreviewListLestSeen
          title="Last watched Recipes"
          :class="{
            RandomRecipes: True,
            blur: !$root.store.username,
            center: true
          }"
          id="lastSeenRecipt"
          disabled
        ></RecipePreviewListLestSeen >
      </b-col> -->
    </b-row>
    
  </div>
</template>

<script>

import RecipePreviewList from "../components/RecipePreviewList";
// import RecipePreviewListLestSeen from "../components/RecipePreviewListLestSeen";
export default {
  components: {
    RecipePreviewList, 
    // RecipePreviewListLestSeen
  },
};

</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin-top: 5vh;
}

#lastSeenRecipt{
  margin-top: 5vh;
  width: 80%;
}

.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(5px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
