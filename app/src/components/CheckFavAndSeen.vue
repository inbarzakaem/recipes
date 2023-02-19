<template>
    <div>
        <div>
            <!-- <div id="favorite" v-if="checkFavIdInArr"> -->
            <div id="favorite" v-if="isFavoriteArr.includes(recipe_id)">
                recipe in favorite
            </div>
            <div v-else>
                <b-button variant="info" @click="addToFavorite(recipe_id)">Click to add recipe to favorite</b-button>

            </div>
        </div>
        <div>
            <div id="seenRecipe" v-if="isSeenArr.includes(recipe_id)">
                    you already see this recipe
                </div > 
                <div v-else>
                    you dont see this recipe
            </div>
        </div>
        

    </div>
</template>

<script>
    export default{
        async mounted() {
            let response1;
            let response2;
            try{
                response1 = await this.axios.get(this.$store.server_domain + "/users/recipes/ids",
                {
                    withCredentials: true,
                });
                this.isSeenArr = response1.data;
            }
            catch(error) {
                console.log(error)
            }
            try{
                response2 = await this.axios.get(this.$store.server_domain + "/users/favorites/ids",
                {
                    withCredentials: true,
                })
                this.isFavoriteArr = response2.data;
            }
            catch(error) {
                console.log(error);
            }
        },
        data() {
            return {
                isSeenArr: [],
                isFavoriteArr: [],
                
            };
        },
        props: {
            recipe_id: {
                required: true
            },
        },
        methods:{
            async addToFavorite(recipe_id){
                try{
                    await this.axios.post(this.$store.server_domain + "/users/favorites/",
                    {
                        recipeId: recipe_id,
                    },
                    {
                        withCredentials: true,
                    });
                    window.location.reload();
                }
                catch(error){
                    console.log(error);
                }

            }
        }
    }
</script>

<style scoped>

</style>