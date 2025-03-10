<template>
  <div class="fondo">
    <div class="row">
      <BotonOpcion texto="Crear evento" @click="crearEvento" tipo="crear" class="col-3" tamano="xx-large" />
      <BotonOpcion texto="Buscar evento" @click="BuscarEvento" tipo="buscar" class="col-3" tamano="xx-large" />
      <BotonOpcion texto="Actualizar evento" @click="actualizarEvento" tipo="actualizar" class="col-3"
        tamano="xx-large" />
      <BotonOpcion texto="Borrar evento" @click="borrarEvento" tipo="borrar" class="col-3" tamano="xx-large" />
    </div>
    <div class="contenedor-crud" v-if="mostrarContenedorCrear">
      <ContenedorCrear />
    </div>
    <div class="contenedor-crud" v-if="mostrarContenedorBuscar">
      <ContenedorBuscar @redirigirActualizarEvento="redigirActualizar" @redirigirBorrarEvento="redigirBorrar" />
    </div>
    <div class="contenedor-crud" v-if="mostrarContenedorActualizar">
      <ContenedorActualizar  />
    </div>
    <div class="contenedor-crud" v-if="mostrarContenedorBorrar">
      <ContenedorBorrar  />
    </div>
    <div
      v-if="!mostrarContenedorCrear && !mostrarContenedorBuscar && !mostrarContenedorActualizar && !mostrarContenedorBorrar">
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
      mostrarContenedorCrear: false,
      mostrarContenedorBuscar: false,
      mostrarContenedorActualizar: false,
      mostrarContenedorBorrar: false,
      eventoSeleccionado: null
    };
  },
  methods: {
    crearEvento() {
      this.mostrarContenedorCrear = true;
      this.mostrarContenedorBuscar = false;
      this.mostrarContenedorActualizar = false;
      this.mostrarContenedorBorrar = false;
    },
    BuscarEvento() {
      this.mostrarContenedorCrear = false;
      this.mostrarContenedorBuscar = true;
      this.mostrarContenedorActualizar = false;
      this.mostrarContenedorBorrar = false;
    },
    actualizarEvento() {
      this.mostrarContenedorCrear = false;
      this.mostrarContenedorBuscar = false;
      this.mostrarContenedorActualizar = true;
      this.mostrarContenedorBorrar = false;
    },
    borrarEvento() {
      this.mostrarContenedorCrear = false;
      this.mostrarContenedorBuscar = false;
      this.mostrarContenedorActualizar = false;
      this.mostrarContenedorBorrar = true;
    },
    redigirActualizar(evento) {
      this.mostrarContenedorCrear = false;
      this.mostrarContenedorBuscar = false;
      this.mostrarContenedorActualizar = true;
      this.mostrarContenedorBorrar = false;
      this.eventoSeleccionado = evento;

    },
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