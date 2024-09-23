<template>
  <div class="text-center">
    <!-- Mensaje de error si la imagen no carga -->
    <v-alert
      v-if="error"
      type="error"
      dismissible
    >
      Error al cargar la imagen. Por favor, inténtalo de nuevo.
    </v-alert>

    <!-- Imagen cargada -->
    <v-img
      v-if="imgLoad"
      :aspect-ratio="16/9"
      class="bg-white mx-auto"
      :src="img"
      width="1000"
      cover
    ></v-img>

    <!-- Indicador de carga -->
    <p v-else>
      Cargando imagen...
      <v-progress-linear
        color="primary"
        indeterminate
      ></v-progress-linear>
    </p>

    <!-- Botón para recargar la imagen -->
    <v-btn class="mt-4" color="primary" @click="loadImage">
      Recargar imagen
    </v-btn>
  </div>
</template>

<script>
  import { ref, onMounted } from 'vue';

  export default {
    setup() {
      const img = ref("");
      const imgLoad = ref(false);
      const error = ref(false);

      const loadImage = async () => {
        imgLoad.value = false;
        error.value = false; // Reiniciar el estado de error
        try {
          const controller = new AbortController();
          const timeoutId = setTimeout(() => {
            controller.abort(); // Abortar si tarda más de 10 segundos
          }, 10000);

          const response = await fetch("https://picsum.photos/1440/1080", {
            signal: controller.signal,
          });

          clearTimeout(timeoutId); // Limpiar el timeout si la imagen se carga a tiempo
          img.value = response.url;
          imgLoad.value = true; // Indica que la imagen está cargada
        } catch (e) {
          error.value = true; // Mostrar alerta de error
        }
      };

      onMounted(loadImage); // Cargar la imagen cuando el componente se monta

      return { img, imgLoad, loadImage, error };
    }
  };
</script>

<style scoped>
  .mx-auto {
    margin-left: auto;
    margin-right: auto;
  }
</style>
