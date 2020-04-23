<template>
<div class="HomePage">
  <b-jumbotron>
<b-container>
  <b-row>
    <b-col>
   <b-form-select v-model="selected" :options="options"> 
     <option class="mt-3" v-for="cats in categories"
        :key="cats"
        :value="cats">{{ cats.name.charAt(0).toUpperCase()+cats.name.slice(1) }}</option>
        </b-form-select> 
        </b-col>
  </b-row>
        <hr class="my-4">
        <b-row>
          <b-col>
   <b-img style="height:400px;width:400px;object-fit: contain;" :src="image.url" rounded="circle" alt="Circle image"></b-img>
    </b-col>
  </b-row>
  <b-row>
    <b-col>
   <b-button @click = "loadNextImage" variant="success" >Next</b-button>
          </b-col>
        </b-row>
</b-container>
   </b-jumbotron>
</div>
</template>
<script>
import axios from "axios";
const url = "https://api.thecatapi.com/v1/categories";
export default {
  name: "HomePage",
  data() {
    return {
      image: { url: "" },
      categories: [],
      selected: null,
        options: [
          { value: null, text: 'Search Breed' }
        ]
    };
  },
 created() {
    this.loadNextImage();
    this.getCategories();
  },
  methods: {
    async loadNextImage() {
      try {
        axios.defaults.headers.common["x-api-key"] =
          "b33872c1-c193-497d-a952-56ef720d5057";

        let response = await axios.get(
          "https://api.thecatapi.com/v1/images/search",
          { params: { limit: 1, size: "full" } }
        );

        this.image = response.data[0]; 

        console.log("-- Image from TheCatAPI.com");
        console.log("id:", this.image.id);
        console.log("url:", this.image.url);
      } catch (err) {
        console.log(err);
      }
    },
  
  async getCategories() {
      await axios
        .get(url)
        .then(res => {
          console.log(res);
          this.categories = [...res.data];
        })
        .catch(err => {
          console.log(err);
          });
    }
  }
};
</script>