<template>
  <h1> {{ titulo }} </h1>
    <v-table v-if="datos" height="600px" fixed-header>
      <thead>
        <tr>
          <th class="text-left">Título</th>
          <th class="text-left">Calificación</th>
          <th class="text-left">IMDB</th>
          <th class="text-left">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="movie,index in datos">
          <td @click="cambiarTitulo(movie.movie)">{{ movie.movie }}</td>
          <td>{{ movie.rating }}</td>
          <td><a :href="movie.imdb_url" target="_blank"> {{movie.imdb_url}} </a></td>
          <td class="d-flex fluid">
            <v-btn icon="mdi-pencil" @click="editar(movie,index)"></v-btn>
            <v-btn icon="mdi-delete" @click="borrar(movie)"></v-btn>
          </td>
        </tr>
      </tbody>
    </v-table>
    <p v-else>Cargando datos...</p>

  <div class="pa-4 text-center"> 
      <v-dialog v-model="dialogEditar" max-width="600">
        <v-card prepend-icon="mdi-pencil" title="Editar película">
          <v-container>
            <v-text-field
              label="Título"
              v-model="datoEditar.movie"
              variant="underlined"
            ></v-text-field>

            <v-text-field
              label="Calificación"
              v-model="datoEditar.rating"
              variant="underlined"
            ></v-text-field>

            <v-text-field
              label="IMDB"
              v-model="datoEditar.imdb_url"
              variant="underlined"
            ></v-text-field>
          </v-container>

          <v-divider></v-divider>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              text="Close"
              variant="plain"
              @click="close"
            ></v-btn>
  
            <v-btn
              color="primary"
              text="Save"
              variant="tonal"
              @click="save"
            ></v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
  </div>

  <div class="text-center pa-4">
    <v-dialog v-model="dialogBorrar" max-width="400" persistent>
      <v-card
        prepend-icon="mdi-delete"
        text="Usted esta de acuerdo de borrar el renglon con la siguiente información:"
        title="¿Desea borrar el renglón?"
      >
        <v-container>
          <v-text-field
            label="Título"
            :model-value="datoBorrar.movie"
            variant="underlined"
            readonly
          ></v-text-field>

          <v-text-field
            label="Calificación"
            :model-value="datoBorrar.rating"
            variant="underlined"
            readonly
          ></v-text-field>

          <v-text-field
            label="IMDB"
            :model-value="datoBorrar.imdb_url"
            variant="underlined"
            readonly
          ></v-text-field>
        </v-container>

        <template v-slot:actions>
          <v-spacer></v-spacer>
          <v-btn @click="btnNoBorrar">Desacuerdo</v-btn>
          <v-btn color="red" @click="btnBorrar">Borrar</v-btn>
        </template>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup>
    import { ref, onMounted} from 'vue';
    const datos = ref(null);
    onMounted( async() => {
        const response = await fetch("https://dummyapi.online/api/movies");
        datos.value = await response.json();
    });

    const titulo = ref('Titulo: ');
    function cambiarTitulo( nombre ){
        titulo.value = `Titulo: ${nombre}`;
    };
    
    /* Dialogo para editar */
    const dialogEditar = ref(false);
    const datoEditar = ref(false);
    const indexEditar = ref(-1); // Guardar el índice del dato que se está editando
    function editar( dato, index ){
      console.log(dato," - ",index);
      datoEditar.value = { ...dato };
      indexEditar.value = index; // Guardar el índice del dato en la lista
      dialogEditar.value = true;
    };

    function save(){
      datos.value[indexEditar.value] = { ...datoEditar.value }; // Reemplazar con los nuevos valores
      dialogEditar.value = false;
    }

    function close(){
      dialogEditar.value = false;
    }

    /* Dialogo para borrar */
    const dialogBorrar = ref(false);
    const datoBorrar = ref(null);

    function borrar( dato ){
      console.log(dato);
      datoBorrar.value = dato;
      dialogBorrar.value = true;
    };

    function btnBorrar(){
      // Filtrar la lista para eliminar el dato seleccionado
      datos.value = datos.value.filter(item => item.id !== datoBorrar.value.id);
      dialogBorrar.value = false;
    }

    function btnNoBorrar(){
      dialogBorrar.value = false;
    }

</script>