<template>
  <v-app>
    <v-content>
      <v-container>
        <div class="text-center ">
        <h1 class="">Consumiendo una API</h1>
        <p>Link de la API publica: <a class="blue--text" href="https://mindicador.cl/" target="_blank" rel="noopener noreferrer">mindicador.cl</a></p>
        <p>Estamos consumiendo una api que nos muestra la cantidad de dolares en dinero chileno dependiendo de el dia seleccionado.</p>

        </div>

        
        <router-view></router-view>

        <!-- LOADER -->
        <v-dialog v-model="loading.estado" hide-overlay persistent width="300">
          <v-card :color="loading.color" dark>
            <v-card-text>
              {{ loading.titulo }}
              <v-progress-linear indeterminate color="white" class="mb-0"></v-progress-linear>
            </v-card-text>
          </v-card>
        </v-dialog>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import { mapState } from "vuex";

export default {
  name: "App",

  components: {},

  data: () => ({
    //
  }),
  computed: {
    ...mapState(["loading"])
  }
};
</script>
