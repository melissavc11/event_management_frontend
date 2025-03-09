<template>
  <div >
    <BotonOpcion texto="Crear evento" @click="crearEvento" tipo="crear"/>
    <BotonOpcion texto="Buscar evento" @click="BuscarEvento" tipo="buscar"/>
    <BotonOpcion texto="Actualizar evento" @click="actualizarEvento" tipo="actualizar"/>
    <BotonOpcion texto="Borrar evento" @click="borrarEvento" tipo="borrar"/>
    <ContenedorCrear v-if= "false"/>
    <ContenedorBuscar v-if= "fasle"/>
    <ContenedorActualizar v-if= "false"/>
    <ContenedorBorrar v-if= "true"/>
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
  }
}
</script>