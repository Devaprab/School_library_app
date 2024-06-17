<template>
  <!-- <v-main>
    <v-container class="py-8 px-6" fluid> -->
  <router-view name="navbar"></router-view>
  <div class="home mt-5 mb-3">
    <h1 class="heading text-center">Welcome to Sanathanam</h1>
    <v-divider></v-divider>
    <v-carousel hide-delimiters>
      <v-carousel-item
        v-for="(item, i) in items"
        :key="i"
        :src="item.src"
        cover
        style="display: inline !important;"
      ></v-carousel-item>
    </v-carousel>
    <h1 class="mt-5 mb-5">Recently Added</h1>
    <div class="d-flex flex-wrap justify-content-center">
      <v-hover v-slot="{ isHovering, props }" v-for="bookList in limitedBookLists" :key="bookList.id">
        <v-card
          class="mx-4 mb-3"
          color="grey-lighten-4"
          width="200"
          v-bind="props"
        >
          <v-img
            :aspect-ratio="1/1.5"
            :src="'data:image/jpeg;base64,' + bookList.image"
            @error="bookList.image = require('@/assets/manju.jpg')"
            cover
          >
            <v-expand-transition>
              <div
                v-if="isHovering"
                class="transition-fast-in-fast-out bg-blue-grey-darken-2 v-card--reveal d-flex"
                style="height: 50%;"
              >
                <p>{{ bookList.bookName }}</p>
              </div>
            </v-expand-transition>
          </v-img>
        </v-card>
      </v-hover>
    </div>
  </div>
  <!-- </v-container>
  </v-main> -->
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      items: [
        {
          src: require('@/assets/green.jpg'),
        },
        {
          src: require('@/assets/sheee.jpg'),
        },
        {
          src: require('@/assets/sll.jpg'),
        },
        {
          src: require('@/assets/book.jpg'),
        },
      ],
      bookLists: [],
    };
  },
  computed: {
    limitedBookLists() {
      return this.bookLists.slice(0, 8);
    },
  },
  mounted() {
    this.getBooks();
  },
  methods: {
    async getBooks() {
      try {
        const response = await axios.get(`${this.$store.getters.getUrl}/library/getAllBooks`);
        if (response.status >= 200 || response.status < 300) {
          console.log(response.data);
          this.bookLists = response.data.sort((a,b) => b.id - a.id);
        }
      } catch (error) {
        console.log(error.message);
        console.error(error);
      }
    }, 
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Barlow:ital@1&family=Charmonman:wght@400;700&family=Jim+Nightshade&display=swap');

.heading {
  font-family: "Charmonman", cursive;
  font-weight: 600;
  font-style: normal;
}

.home {
  height: auto;
  width: 1200px;
}

.v-card--reveal {
  align-items: center;
  bottom: 0;
  justify-content: center;
  opacity: .9;
  position: absolute;
  width: 100%;
}
</style>
