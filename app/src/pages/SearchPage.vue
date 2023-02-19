<template>
  <div class="container">
    <h1 class="title">Search Page</h1>
    <br />
    <b-form @submit.prevent="onSearch" @reset.prevent="onReset">
      <table>
        <!-- Query Field -->
        <tr>
          <th>
            <b-form-group id="input-group-query" label="Search:" label-for="query">
              <b-form-input
                id="query"
                v-model="$v.form.query.$model"
                type="text"
                class="mb-3"
                :state="validateState('query')"
                >
              </b-form-input>
            </b-form-group>
          </th>

          <!-- Result No -->
          <th>
            <b-form-group id="input-group-number" label="Number of Result:" label-for="number"></b-form-group>
            <b-form-select
              id="number"
              class="mb-3"
              v-model="$v.form.number.$model"
              :options="optResultsNo"
              :state="validateState('number')"
            >
              <template v-slot:first>
                <b-form-select-option :value="null" disabled>-- Please select an option --</b-form-select-option>
              </template>
            </b-form-select>
            <b-form-invalid-feedback v-if="!$v.form.number.required">Result Number is required</b-form-invalid-feedback>
          </th>

          <!-- Cuisine -->
          <th>
            <b-form-group id="input-group-cuisine" label="Cuisine:" label-for="cuisine"></b-form-group>
              <b-dropdown text="Choose Cuisines" >
                <b-form-checkbox-group v-model="selectedCuisines" :options="optCuisines" stacked>
                </b-form-checkbox-group>
              </b-dropdown>
            <b-form-invalid-feedback v-if="!$v.form.cuisine.required">Result Number is required</b-form-invalid-feedback>
          </th>

          <!-- Diet -->
          <th>
            <b-form-group id="input-group-diet" label="Diet:" label-for="diet"></b-form-group>
            <b-dropdown text="Choose Diet" >
              <b-form-checkbox-group v-model="selectedDiets" :options="optDiets" stacked>
              </b-form-checkbox-group>
            </b-dropdown>
            <b-form-invalid-feedback v-if="!$v.form.diet.required">Result Number is required</b-form-invalid-feedback>
          </th>

          <!-- Intolarence -->
          <th>
            <b-form-group id="input-group-intolarence" label="Intolarence:" label-for="intolarence"></b-form-group>
            <b-dropdown text="Choose Intolarences" >
              <b-form-checkbox-group v-model="selectedIntolarences" :options="optIntolarences" stacked>
              </b-form-checkbox-group>
            </b-dropdown>
            <b-form-invalid-feedback v-if="!$v.form.diet.required">Intolarence is required</b-form-invalid-feedback>
          </th>

          <!-- Reset & Submit -->
          <th>
            <b-button class="b-button" type="submit" variant="primary">Search</b-button>
          </th>
          <th>
            <b-button class="b-button" type="reset" variant="danger">Reset</b-button>
          </th>
        </tr>
        <tr>
          <p style="color: red;" v-if="!$v.form.query.required && submitted">Text is required</p>
        </tr>
      </table>
      <br>
    </b-form>
    
    <div v-if="submitted && results.length == 0">
      <span><center><strong> could not find any results</strong></center></span>
      <br>
    </div>
    <!-- SORT SECTION -->
    <div v-if="results.length > 0">
      <label class="sort">Sort By: </label>
      <select v-model="sortParam">
        <option>Popularity</option>
        <option>Time</option>
      </select>
      <label class="sort">Order By: </label>
      <select v-model="orderParam">
        <option>Ascending</option>
        <option>Descending</option>
      </select>
      <b-button class="b-button" type="submit" @click="sort" variant="primary">Sort Results</b-button>
      <ul>
        <ol v-for="result in results" v-bind:key="result.id" class="result">
          <RecipePreview class="recipePreview" :recipe="result" />
          <CheckFavAndSeenVue :recipe_id="result.id"></CheckFavAndSeenVue>
        </ol>
      </ul>
    </div>
  </div>
</template>

<script>
import countries from "../assets/countries";
import Cuisines from "../assets/cuisine";
import Diets from "../assets/diet";
import Intolerances from "../assets/intolerance";
import RecipePreview from "../components/RecipePreview";
import CheckFavAndSeenVue from "../components/CheckFavAndSeen.vue";

import {
  required,
  minLength,
  maxLength,

} from "vuelidate/lib/validators";

export default {
  components: {
    RecipePreview,
    CheckFavAndSeenVue,
  },
  name: "Search",
  data() {
    return {
      selectedIntolarences: [],
      selectedDiets: [],
      selectedCountry: [],
      selectedCuisines: [],
      resultNumbers:[5, 10, 15],
      form: {
        query: "",
        number: "5",
        cuisine: "",
        diet: "",
        intolarence: ""
      },
      validated: false,
      results: [],
      optCuisines: [],
      optIntolarences: [],
      countries: [],
      optDiets: [],
      optResultsNo: [],
      errors: [],
      sortParam: "Popularity",
      orderParam: "Ascending",
      submitted: false
    };
  },
  validations: {
    form: {
      query: {
        required,
        length: u => minLength(1)(u) && maxLength(20)(u)
      },
      number: {
        // required
      },
      cuisine: {
        // required
      },
      diet: {
        // required
      },
      intolarence: {
        // required
      }
    }
  },
  mounted() {
    this.countries.push(...countries);
    this.optCuisines.push(...Cuisines);
    this.optDiets.push(...Diets);
    this.optIntolarences.push(...Intolerances);
    this.optResultsNo.push(...this.resultNumbers);
    if (sessionStorage.getItem("lastQuery") != null) {
      console.log("got inside the load previous search");
      this.form.query = sessionStorage.getItem("lastQuery");
      this.results = JSON.parse(sessionStorage.getItem("lastResults"));
    }
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search() {
      console.log("entered search");
      let response;
      try {
        let params = {};
        params.query = this.form.query;
        params.number = this.form.number
        if (this.selectedCuisines.length > 0) {
          params.cuisine = this.selectedCuisines.toString();
        }
        if (this.selectedDiets.length > 0) {
          params.diet = this.selectedDiets.toString();
        }
        if (this.selectedIntolarences.length > 0) {
          params.intolarence = this.selectedIntolarences.toString();
        }
        response = await this.axios.get(
          this.$store.server_domain + "/recipes/search/",
          {
            params: params
          },
          {
            withCredentials: true
          }
        );

        if (response.status !== 200){
          this.$router.replace("/NotFound");
        } 
        else this.results.push(...response.data);
        console.log("save to lastQuery this value: ", this.form.query);
        sessionStorage.setItem("lastQuery", this.form.query);
        console.log("save to lastResults this value: ", this.results);
        sessionStorage.setItem("lastResults", JSON.stringify(this.results));

        this.submitted = true;
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
    },
    onSearch() {
      this.results = [];
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      this.Search();
    },
    onReset() {
      this.form = {
        query: "",
        number: "5",
        cuisine: "",
        diet: "",
        intolarence: ""
      };
      this.results = [],
      this.$nextTick(() => {
        this.$v.$reset();
      });
    },
    sort() {
      let x;
      if(this.sortParam == "Popularity") {
        if(this.orderParam == "Ascending") {
          x = this.results.sort(function(a, b) {
            return a.aggregateLikes - b.aggregateLikes;
          })
          this.results = x;
        } else if(this.orderParam == "Descending") {
          x = this.results.sort(function(a, b) {
            return b.aggregateLikes - a.aggregateLikes;
          })
        }
      }
      else if(this.sortParam == "Time") {
        if(this.orderParam == "Ascending") {
          x = this.results.sort(function(a, b) {
            return a.readyInMinutes - b.readyInMinutes;
          })
        } else if(this.orderParam == "Descending") {
          x = this.results.sort(function(a, b) {
            return b.readyInMinutes - a.readyInMinutes;
          })
        }
      }
      this.results = x;
      sessionStorage.setItem("lastResults", JSON.stringify(this.results));
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  max-width: auto;
  background-color: #EFEFEF;
  border-radius: 25px;
  border-style: solid;
  border-color: darkgrey;
  padding: 20px;
}

.result {
  max-width: 55%;
}

.dropdown.dropdown-scroll .dropdown-list{
    max-height: 233px;
    overflow-y: auto;
    list-style-type: none;
    padding-left: 10px;
    margin-bottom: 0px;
}
.dropdown-list  li{
    font-size: 14px;
    height: 20px;
}

.dropdown-list  li > a{
    color: black;
}
.dropdown-list a:hover{
   color: black;
}

.dropdown-list li:hover{
    background-color: lightgray;
}
.sort {
  padding-left :10px;
  padding-right :10px;
}
.b-button {
  margin: 10px;
}
</style>