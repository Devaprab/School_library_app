<template>
  <v-app class="library">
    <v-main>
      <div>
        <router-view></router-view>
      </div>
      
    </v-main>
  </v-app>
</template>

<style>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.library {
  /* background-image: url('@/assets/libraryImg.jpg'); */
  background-size: cover; 
  background-position: center;
  height: 100%;
  object-fit: cover;
}

</style>
