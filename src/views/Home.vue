<template>
  <div>
    <v-layout :wrap="true">
      <v-flex xs12>
        <v-card>
          <v-date-picker
            v-model="fecha"
            full-width
            locale="es-mx"
            :min="minimo"
            :max="maximo"
            @change="getDolar(fecha)"
          ></v-date-picker>
        </v-card>
        <v-card color="error" dark>
          <v-card-text class="display-1 text-sm-center">{{ valor }}</v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
import { mapMutations } from "vuex";

export default {
  name: "home",
  components: {},
  data() {
    return {
      fecha: new Date()
        .toISOString()
        .substr(
          0,
          10
        ) /* Esta es la fecha que seleccionara el cliente pero por defecto dejamos la fecha que tiene configurada */,
      minimo:
        "1984" /* Esta es la fecha minima en la que el usuario podra seleccionar */,
      maximo: new Date()
        .toISOString()
        .substr(
          0,
          10
        ) /* Con esto creamos un objeto Date que nos trae la fecha que el usuario tiene configurada */,
      valor: null /* Aqui se almacenara el valor del dolar en el dia especifico */
    };
  },
  methods: {
    ...mapMutations(["mostrarLoading", "ocultarLoading"]),

    async getDolar(dia) {
      // Invertimos el dia
      // console.log(dia);
      let arrayFecha = dia.split([
        "-"
      ]); /* Lo que hace esto es separa nuestra cadena en un array por el separador del guion */
      // console.log(arrayFecha);
      let ddmmyy = arrayFecha[2] + "-" + arrayFecha[1] + "-" + arrayFecha[0];
      // console.log(ddmmyy);

      try {
        const state = {
          titulo: "Accediendo a informacion",
          color: "secondary"
        };
        this.mostrarLoading(state);

        // El uso de async y await quiere decir que datos va a esperar que la peticion de axios se termine de ejecutar
        let datos = await axios.get(
          `https://mindicador.cl/api/dolar/${ddmmyy}`
        );
        // // console.log(datos.data.serie[0].valor);
        if (datos.data.serie.length > 0) {
          this.valor = await datos.data.serie[0].valor;
        } else {
          this.valor = "Sin Resultados";
        }
      } catch (error) {
        //console.log(error)
      } finally {
        this.ocultarLoading();
      }
    }
  },
  created() {
    this.getDolar(this.fecha);
  }
};
</script>
