<template>
    <v-container>
      <!-- Imagen cargada -->
      <v-img
        v-if="imageLoaded && !error"
        :src="imageUrl"
        max-width="1000"
        :aspect-ratio="16/9"
      ></v-img>
  
      <!-- Mensaje de error si la imagen no carga -->
      <v-alert
        v-if="error"
        type="error"
        dismissible
      >
        Error al cargar la imagen. Por favor, inténtalo de nuevo.
      </v-alert>
  
      <!-- Indicador de carga -->
      <v-progress-linear
        v-if="loading"
        indeterminate
        color="primary"
      ></v-progress-linear>
  
      <!-- Botón para cargar la siguiente imagen -->
      <v-btn @click="loadImage" :disabled="loading">
        Siguiente
      </v-btn>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
      return {
        imageUrl: '',       // URL de la imagen actual
        loading: false,     // Estado de carga
        error: false,       // Estado de error
        imageLoaded: false, // Estado de imagen cargada correctamente
      };
    },
    methods: {
      async loadImage() {
        this.loading = true;
        this.error = false;
        this.imageLoaded = false;
  
        try {
          // Simulación de carga de imagen con un API (ej: https://picsum.photos/1440)
          const response = await fetch('https://picsum.photos/1440');
          
          if (!response.ok) {
            throw new Error('Error al cargar la imagen');
          }
  
          // Actualiza la URL de la imagen cuando se cargue correctamente
          this.imageUrl = response.url;
          this.imageLoaded = true;
        } catch (e) {
          this.error = true; // Manejar error de carga
        } finally {
          this.loading = false;
        }
      },
    },
  };
  </script>