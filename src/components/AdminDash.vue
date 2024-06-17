<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" elevation="4" app>
      <v-sheet class="pa-4 pb-0 bg-light-green-lighten-5 d-flex justify-content-center flex-column align-items-center">
        <v-avatar class="mb-2" color="light-green-darken-4" size="64">
          <v-icon class="mdi mdi-book-open-page-variant-outline"></v-icon>
        </v-avatar>
        <p class="fs-4 ms-2">Sanathanam</p>
      </v-sheet>
      <v-list density="compact" nav class="mt-0">
        <v-list-item value="View Books" class="navlist fs-6" style="border-bottom: 1px solid #E0E0E0;" 
          @click="$router.push({ name: 'books' })">
          View Books
        </v-list-item>
        <v-list-item value="Manage Books" class="navlist fs-6" style="border-bottom: 1px solid #E0E0E0;" 
          @click="$router.push({ name: 'managebook' })">
          Manage Books
        </v-list-item>
        <v-list-item value="Add books" class="navlist fs-6" style="border-bottom: 1px solid #E0E0E0;" 
          @click="$router.push({ name: 'addbook' })">
          Add Books
        </v-list-item>
        <v-list-item value="issue-book" class="navlist fs-6" style="border-bottom: 1px solid #E0E0E0;"
          @click="$router.push({ name: 'bookissue' })">
          Issue Books
        </v-list-item>
        <v-list-item value="view-teachers" class="navlist fs-6" style="border-bottom: 1px solid #E0E0E0;"
          @click="$router.push({ name: 'teachers' })">
          View Teachers
        </v-list-item>
        <v-list-item value="returned-book" class="navlist fs-6" style="border-bottom: 1px solid #E0E0E0;"
          @click="$router.push({ name: 'returnedBooks' })">
          Returned Books
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar class="green-background py-1" app>
      <v-app-bar-nav-icon @click="drawer = !drawer" color="black"></v-app-bar-nav-icon>
      <v-container fluid class="d-flex justify-content-between align-items-center ps-0 ms-0">
        <h5 class="d-inline mb-0 text-black">{{ selectedNavItem }}</h5>
        <v-btn class="rounded-2 text-capitalize" @click="logout" prepend-icon="mdi-logout" color="black" variant="elevated">
          Logout
        </v-btn>
      </v-container>
    </v-app-bar>

    <v-main class="main-content mt-5 pe-5">
      <v-container fluid>
        <router-view></router-view>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: true,
      rail: true,
      navItems: [ 
        { text: 'View Books', route: '/books' },
        { text: 'Manage Books', route: '/managebook' },
        { text: 'Add Books', route: '/addbook' },
        { text: 'Issue Books', route: '/bookissue' },
        { text: 'View Teachers', route: '/teachers' },
        { text: 'Returned books', route: '/returnedBooks' }
      ],
    }
  },
  computed: {
    selectedRoute() {
      return this.$route.path;
    },
    selectedNavItem() {
      let selectedItem = null;
      for (let i = 0; i < this.navItems.length; i++) {
        if (this.selectedRoute.startsWith(this.navItems[i].route)) {
          selectedItem = this.navItems[i];
          break;
        }
      }
      return selectedItem ? selectedItem.text : '';
    },
  },
  methods: {
    logout() {
      this.$router.push({ name: 'login' });
    },
    navigate(route) {
      if (this.status) {
        this.$router.push(route);
      }
    },
  },
  watch: {
    selectedRoute(newRoute) {
      let selectedItem = null;
      for (let i = 0; i < this.navItems.length; i++) {
        if (newRoute.startsWith(this.navItems[i].route)) {
          selectedItem = this.navItems[i];
          break;
        }
      }
      if (selectedItem) {
        this.selectedNavItem = selectedItem.text;
      }
    }
  }
};
</script>

<style scoped>
.green-background {
  background-image: linear-gradient(0deg, rgba(192, 241, 66, 0.2) 0%, rgba(192, 241, 66, 0.2) 16.667%, rgba(73, 212, 31, 0.2) 16.667%, rgba(73, 212, 31, 0.2) 33.334%, rgba(113, 222, 43, 0.2) 33.334%, rgba(113, 222, 43, 0.2) 50.001%, rgba(232, 251, 78, 0.2) 50.001%, rgba(232, 251, 78, 0.2) 66.668%, rgba(152, 231, 54, 0.2) 66.668%, rgba(152, 231, 54, 0.2) 83.335%, rgba(33, 202, 19, 0.2) 83.335%, rgba(33, 202, 19, 0.2) 100.002%), linear-gradient(135deg, rgba(192, 241, 66, 0.2) 0%, rgba(192, 241, 66, 0.2) 16.667%, rgba(73, 212, 31, 0.2) 16.667%, rgba(73, 212, 31, 0.2) 33.334%, rgba(113, 222, 43, 0.2) 33.334%, rgba(113, 222, 43, 0.2) 50.001%, rgba(232, 251, 78, 0.2) 50.001%, rgba(232, 251, 78, 0.2) 66.668%, rgba(152, 231, 54, 0.2) 66.668%, rgba(152, 231, 54, 0.2) 83.335%, rgba(33, 202, 19, 0.2) 83.335%, rgba(33, 202, 19, 0.2) 100.002%), linear-gradient(90deg, rgba(192, 241, 66, 0.2) 0%, rgba(192, 241, 66, 0.2) 16.667%, rgba(73, 212, 31, 0.2) 16.667%, rgba(73, 212, 31, 0.2) 33.334%, rgba(113, 222, 43, 0.2) 33.334%, rgba(113, 222, 43, 0.2) 50.001%, rgba(232, 251, 78, 0.2) 50.001%, rgba(232, 251, 78, 0.2) 66.668%, rgba(152, 231, 54, 0.2) 66.668%, rgba(152, 231, 54, 0.2) 83.335%, rgba(33, 202, 19, 0.2) 83.335%, rgba(33, 202, 19, 0.2) 100.002%), linear-gradient(90deg, rgb(238, 207, 232), rgb(127, 195, 255)); 
}
a.router-link-active .v-list-item,
.v-list-item--active {
  background-color: #43A047;
  text-decoration: none;
  color: white;
}
.navlist {
  padding: 12px 64px;
}
</style>
