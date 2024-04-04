<template>
  <v-app-bar :elevation="0">
    <template v-slot:prepend>
      <v-app-bar-nav-icon @click="toggleDrawer"></v-app-bar-nav-icon>
    </template>

    <v-app-bar-title>Application Bar</v-app-bar-title>
  </v-app-bar>
  <v-navigation-drawer v-model="drawer" app>
    <v-list>
      <v-list-item link :to="{ name: '/' }">Inicio</v-list-item>
      <v-list-item link :to="{ name: '/imagen' }">Imagen</v-list-item>
      <v-list-item link :to="{ name: '/tabla' }">Tabla</v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const drawer = ref(false);

    function toggleDrawer() {
      drawer.value = !drawer.value;
    }

    return {
      drawer,
      toggleDrawer,
    };
  },
};
</script>
