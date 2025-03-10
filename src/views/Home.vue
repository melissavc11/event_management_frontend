<template>
  <div class="fondo">
    <div class="row">
      <!-- @component BotonOpcion - Botón para crear un evento -->
      <BotonOpcion texto="Crear evento" @click="crearEvento" tipo="crear" class="col-3" tamano="xx-large" />
      <!-- @component BotonOpcion - Botón para buscar un evento -->
      <BotonOpcion texto="Buscar evento" @click="BuscarEvento" tipo="buscar" class="col-3" tamano="xx-large" />
      <!-- @component BotonOpcion - Botón para actualizar un evento -->
      <BotonOpcion texto="Actualizar evento" @click="actualizarEvento" tipo="actualizar" class="col-3" tamano="xx-large" />
      <!-- @component BotonOpcion - Botón para borrar un evento -->
      <BotonOpcion texto="Borrar evento" @click="borrarEvento" tipo="borrar" class="col-3" tamano="xx-large" />
    </div>
    <div class="contenedor-crud" v-if="mostrarContenedorCrear">
      <!-- @component ContenedorCrear - Contenedor para crear un evento -->
      <ContenedorCrear />
    </div>
    <div class="contenedor-crud" v-if="mostrarContenedorBuscar">
      <!-- @component ContenedorBuscar - Contenedor para buscar un evento -->
      <ContenedorBuscar @redirigirActualizarEvento="redigirActualizar" @redirigirBorrarEvento="redigirBorrar" />
    </div>
    <div class="contenedor-crud" v-if="mostrarContenedorActualizar">
      <!-- @component ContenedorActualizar - Contenedor para actualizar un evento -->
      <ContenedorActualizar />
    </div>
    <div class="contenedor-crud" v-if="mostrarContenedorBorrar">
      <!-- @component ContenedorBorrar - Contenedor para borrar un evento -->
      <ContenedorBorrar />
    </div>
    <div v-if="!mostrarContenedorCrear && !mostrarContenedorBuscar && !mostrarContenedorActualizar && !mostrarContenedorBorrar">
      <!-- @display Image - Imagen a mostrar cuando no hay contenedores visibles -->
      <img src="@/assets/icono.png" alt="Description of the image" style=" padding-top: 5%;">
    </div>
  </div>
</template>

<script>
import ContenedorCrear from '@/components/Contenedores/ContenedorCrear.vue';
import ContenedorBuscar from '@/components/Contenedores/ContenedorBuscar.vue';
import ContenedorActualizar from '@/components/Contenedores/ContenedorActualizar.vue';
import ContenedorBorrar from '@/components/Contenedores/ContenedorBorrar.vue';
import BotonOpcion from '@/components/BotonOpcion.vue';

export default {
  name: 'HomeView',
  components: {
    ContenedorCrear,
    ContenedorBuscar,
    ContenedorActualizar,
    ContenedorBorrar,
    BotonOpcion
  },
  data() {
    return {
      /** 
       * @description Muestra el contenedor para crear un evento 
       * @type {boolean}
       */
      mostrarContenedorCrear: false,
      /** 
       * @description Muestra el contenedor para buscar un evento 
       * @type {boolean}
       */
      mostrarContenedorBuscar: false,
      /** 
       * @description Muestra el contenedor para actualizar un evento 
       * @type {boolean}
       */
      mostrarContenedorActualizar: false,
      /** 
       * @description Muestra el contenedor para borrar un evento 
       * @type {boolean}
       */
      mostrarContenedorBorrar: false,
      /** 
       * @description Evento seleccionado para actualizar o borrar 
       * @type {object|null}
       */
      eventoSeleccionado: null
    };
  },
  methods: {
    /**
     * @descriptionuestra el contenedor para crear un evento y oculta los demás
     */
    crearEvento() {
      this.mostrarContenedorCrear = true;
      this.mostrarContenedorBuscar = false;
      this.mostrarContenedorActualizar = false;
      this.mostrarContenedorBorrar = false;
    },
    /**
     * @description Muestra el contenedor para buscar un evento y oculta los demás
     */
    BuscarEvento() {
      this.mostrarContenedorCrear = false;
      this.mostrarContenedorBuscar = true;
      this.mostrarContenedorActualizar = false;
      this.mostrarContenedorBorrar = false;
    },
    /**
     * @description Muestra el contenedor para actualizar un evento y oculta los demás
     */
    actualizarEvento() {
      this.mostrarContenedorCrear = false;
      this.mostrarContenedorBuscar = false;
      this.mostrarContenedorActualizar = true;
      this.mostrarContenedorBorrar = false;
    },
    /**
     * @description Muestra el contenedor para borrar un evento y oculta los demás
     */
    borrarEvento() {
      this.mostrarContenedorCrear = false;
      this.mostrarContenedorBuscar = false;
      this.mostrarContenedorActualizar = false;
      this.mostrarContenedorBorrar = true;
    },
    /**
     * @description Redirige al contenedor de actualizar evento con el evento seleccionado
     * @param {object} evento - El evento seleccionado para actualizar     */
    redigirActualizar(evento) {
      this.mostrarContenedorCrear = false;
      this.mostrarContenedorBuscar = false;
      this.mostrarContenedorActualizar = true;
      this.mostrarContenedorBorrar = false;
      this.eventoSeleccionado = evento;
    },
    /**
     * @description Redirige al contenedor de borrar evento con el evento seleccionado
     * @param {object} evento - El evento seleccionado para borrar
     */
    redigirBorrar(evento) {
      this.mostrarContenedorCrear = false;
      this.mostrarContenedorBuscar = false;
      this.mostrarContenedorActualizar = false;
      this.mostrarContenedorBorrar = true;
      this.eventoSeleccionado = evento;
    }
  }
}
</script>

<style scoped>
.contenedor-crud {
  margin: 2% 10% 2% 10%;
  min-height: 100%;
  /* border: 1px solid #09B86C; */
}

.fondo {
  margin: 0%;
  padding: 5%;
}
</style>
