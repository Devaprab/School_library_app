<template>
  <v-main>
    <v-container justify-center>
    <div class="d-flex flex-column">
        <!-- <v-chip-group
        selected-class="text-primary"
        >
  <v-chip>English</v-chip>

  <v-chip>Hindi</v-chip>

  <v-chip>Malayalam</v-chip>
</v-chip-group> -->
    
    <div class="d-flex flex-wrap">
    <div v-for="bookList in bookLists" :key="bookList.id" class="mx-3 my-3">
       
    <v-card
      class="mx-auto pb-3"
      width="200"
    >
      <v-img
        :aspect-ratio="1/1.5"
        :src="bookList.image"
        cover
      ></v-img>
  
      <v-card-title>
        {{ bookList.bookName }}
      </v-card-title>
  
      <v-card-subtitle>
        {{ bookList.authorName }}
      </v-card-subtitle>
      <v-card-subtitle>
        Rack : {{ bookList.rack }}
      </v-card-subtitle>
      <v-card-subtitle>
        Quantity : {{ bookList.quantity }}
      </v-card-subtitle>
    </v-card>
</div>
</div>
</div>
</v-container>
</v-main>
  </template>

<script>
import axios from 'axios';
// v-for="bookList in bookLists" :key="bookList.id" 
import { mapGetters } from 'vuex'
export default {
    computed: {
      ...mapGetters(['displayBooks']),
      bookLists() {
        return this.displayBooks;
      },
    },
    mounted() {
      this.getBooks();
    },
    methods : {
      async getBooks() {
        try{
       const response = await axios.get(`${this.$store.getters.getUrl}/library/getAllBooks`);
           if(response.status >= 200 || response.status < 300){
            console.log(response.data);
            this.$store.commit('setBooks',response.data);
           }
          }catch(error) {
            console.log(error.message);
            console.error(error);
          }
      }
    }
};
</script>