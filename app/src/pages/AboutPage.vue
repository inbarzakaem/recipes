<template>
  <div class="container">
    <!-- <h1 class="title">About Page</h1> -->
    <!-- <br /> -->
      <b-container fluid class="bg">
    <h4 class="titles">About Our Website</h4>
     <p>
       The site is about delicous Recipes, which includes for each recipe the functionality of:
       <br>
      <ul>
      <li>Image of the Recipe.</li>
      <li>The needed ingredients for making the recipe.</li>
      <li>The amount of each ingredient.</li>
      <li>Guided steps for how to make the recipe properly.</li>
      </ul>
     Also, every visitor can explore new recipes at the home page and also search for recipes that match his falvors in the search page.
     <br>
     <br>
     Visitors who register to our website can also enjoy:
      <ul>
      <li>Mark recipes that they liked and they will be saved for them at the 'My Favorite Recipes'.</li>
      <li>An inidication for recipes that he already seen before.</li>
      <li>The amount of each ingredient.</li>
      </ul>
       </p>
    <h4 class="titles">About The Developers</h4>
    <p>
     
      <br>
      <br>
      <u>
      Our team:
      </u>
      <br>
      <table>
        <tr>
          <th>

          </th>
          <th>
            <b>
              Shahr hadad- 
            </b>
            
            <ul>
              <li>ID: 201659190 </li>
              <li>Email:hshah@post.bgu.ac.il</li>
            </ul>
          </th>
        </tr>
      </table>
      <table>
        <tr>
          <th>
            
          </th>
          <th>
            <b>
            Avichai Edri - 
            </b>
            
            <ul>
              <li>ID: 316157379</li>
              <li>Email:avichaie@post.bgu.ac.il</li>
            </ul>
          </th>
        </tr>
      </table>
            <table>
        <tr>
          <th>

          </th>
          <th>
            <b>
              Inbar Zakaem- 
            </b>
            
            <ul>
              <li>ID: 308140649</li>
              <li>Email: inbarza@post.bgu.ac.il</li>
            </ul>
          </th>
        </tr>
      </table>
    </p>
    <h4 class="titles">Checkout Our other Sites:</h4>
    </b-container>
  </div>
</template>

// <script>
export default {
  name: "about",
  data() {
    return {
      
    };
  }
};
</script>

<style scoped>
.container {
  max-width: 80%;
}

.site-image {
  border-radius: 150px;
  padding: 10px;
  margin: 5px;
  filter: grayscale(45%);
  transition: all 0.7s ease-out;
}
.site-image:hover {
  border: 1px solid #777;
  filter: grayscale(0);
  transform: scale(1.01211352033886652036768955);
  transition-duration: 0.7s;
}
.titles {
  font-weight: bold;
  text-decoration: underline;
}

.bg
{
  background-color: #EFEFEF;
  border-radius: 25px;
  border-style: solid;
  border-color: darkgrey;
  padding: 20px;
}
.developer-images {
  width: 150px;
  height: 150px;
  border-radius: 50%;
}
</style>
