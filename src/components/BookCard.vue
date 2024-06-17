<template>
  <!-- <v-main>
    <v-container fluid> -->
    <div class="d-flex flex-wrap justify-content-center">
      <div v-if="loading" class="d-flex flex-wrap gap-4 justify-content-center">
      <div v-for="n in 8" :key="n">
       <v-skeleton-loader
       
          class="mx-auto border"
          width="250"
          type="image, article"
        ></v-skeleton-loader> 
      </div>
    </div>
  <v-hover v-else v-slot="{ isHovering, props }" v-for="bookList in bookLists" :key="bookList.id">
    <v-card 
      class="mx-4 mb-3"
      color="grey-lighten-4"
      width="200"
      v-bind="props"
    >
      <v-img
        :aspect-ratio="1/1.5"
        :src="'data:image/jpeg;base64,' + bookList.image"
        cover
    
      >
        <v-expand-transition>
          <div
            v-if="isHovering"
            class="transition-fast-in-fast-out bg-green-lighten-3 v-card--reveal px-2"
            style="height: 100%;"
          >
            <h5 class="pt-2">{{ bookList.bookName }}</h5>
            <p>Author : <span>{{ bookList.authorName }}</span></p>
            <p>Category : <span>{{ bookList.category }}</span></p>
            <p>Rack : <span>{{ bookList.rack }}</span></p>
            <p>Quantity : <span>{{ bookList.quantity }}</span></p>
          </div>
        </v-expand-transition>
      </v-img>
    </v-card>
  </v-hover>
</div>
<!-- </v-container>
</v-main> -->
</template>

<script>
import axios from 'axios';
// v-for="bookList in bookLists" :key="bookList.id" 
import { mapGetters } from 'vuex'
export default {
  data() {                                
        return {
            loading: true,
        }
    },
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
        this.loading = false;
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