<template>
  <v-app>
    <v-layout>
      <v-app-bar prominent>
        <v-app-bar-nav-icon variant="text" @click="onDrawer"></v-app-bar-nav-icon>
        <v-toolbar-title>vuetify</v-toolbar-title>
      </v-app-bar>

      <v-navigation-drawer v-model="drawer">
        <v-list-item title="My Application" subtitle="Vuetifyyy"></v-list-item>
        <v-divider></v-divider>
        <v-list>
          <v-list-item prepend-icon="mdi-view-dashboard" title="Inicio" to="/"></v-list-item>
          <v-list-item prepend-icon="mdi-card-text" title="Card" to="/cardpage"></v-list-item>
          <v-list-item prepend-icon="mdi-movie" title="Peliculas" to="/peliculaspage"></v-list-item>
        </v-list>
      </v-navigation-drawer>

      <v-main>
        <router-view />
      </v-main>
      <AppFooter />
    </v-layout>
  </v-app>
</template>

<script setup>
import { ref } from 'vue';
const drawer = ref(false);

function onDrawer(){
  drawer.value = !drawer.value
}

</script>