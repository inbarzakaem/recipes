<template>
  <div id="app">
    <div>
      <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-brand :to="{ name: 'main' }">Main Recipes</b-navbar-brand>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item :to="{ name: 'about' }">About</b-nav-item>
            <b-nav-item :to="{ name: 'search' }">Search</b-nav-item>
            <span v-if="$root.store.username">

              <b-nav-item-dropdown >
              <template v-slot:button-content>
                <em>Personal</em>
              </template>
              <!-- open model box -->
              <b-dropdown-item> <router-link :to="{ name: 'myFavorites' }">MyFavorites</router-link> </b-dropdown-item>
              <b-dropdown-item>  <router-link :to="{ name: 'familyRecipes' }">FamilyRecipes</router-link> </b-dropdown-item>
              <b-dropdown-item> <router-link :to="{ name: 'myRecipes' }">MyRecipes</router-link> </b-dropdown-item>
              </b-nav-item-dropdown>            
            </span>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">

          <!-- Drop Down button -->
          <span v-if="!$root.store.username">
            <!-- Guest section -->
            <b-nav-item-dropdown right>
              <template v-slot:button-content>
                <em>hello guest</em>
              </template>
              <b-dropdown-item>
                <router-link :to="{ name: 'register' }">Register</router-link>
              </b-dropdown-item>

              <b-dropdown-item> 
              <router-link :to="{ name: 'login' }">Login</router-link>
              </b-dropdown-item> 
            </b-nav-item-dropdown>
          </span>
            <div v-else>
              <!-- User section -->
              <b-nav-item-dropdown >
              <template v-slot:button-content>
                <em>Welcome {{ $root.store.username }}</em>
              </template>
              <!-- open model box -->
              <b-dropdown-item v-b-modal.recipeCreation>Create Recipe</b-dropdown-item>
              <b-dropdown-item @click="Logout">Logout</b-dropdown-item>
            </b-nav-item-dropdown>
            </div>
          </b-navbar-nav>
        </b-collapse>
        

      </b-navbar>
    </div>
    <createRecipeModelVue id="recipeCreation"></createRecipeModelVue>
    <router-view />
  </div>
  
</template>

<script>
import createRecipeModelVue from './components/createRecipeModel.vue';
export default {
  name: "App",
  components: {
    createRecipeModelVue: createRecipeModelVue
  },
  data(){
    return{

    }
  },
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>