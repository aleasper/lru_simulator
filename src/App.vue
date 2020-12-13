<template>
  <v-app>
    <v-app-bar
        app
        color="primary"
        dark
    >
      <div class="app-bar-title">
        <h1>LRU</h1>
      </div>
      <div id="nav">
        <router-link to="/" class="app-bar-link">К приложению</router-link> |
        <router-link to="/about" class="app-bar-link">О программе</router-link> |
        <router-link to="/theory" class="app-bar-link">Теория</router-link>
      </div>
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>


export default {
  name: 'App',

  components: {

  },

  data: () => ({
    //
  }),
};
</script>

<style scoped>
#nav{
  color: white;
}
.app-bar-link{
  color: white;
}
.app-bar-title{
  margin: 0 40px 0 0;
}
</style>
