<template>
    <div>
        <ListaDesplegable v-model="eventoSeleccionado" :opciones="opcionesEventos" etiqueta="Eventos" />
        <BotonOpcion @click="borrarEvento" texto="Borrar" tipo="aceptar"/>
        <BotonOpcion @click="limpiarDatos" texto="Cancelar" tipo="cancelar"/>
    </div>
</template>

<script>
import ListaDesplegable from '../ListaDesplegable.vue';
import BotonOpcion from '../BotonOpcion.vue';
export default {
    name: 'ContenedorBorrar',
    components: {
        ListaDesplegable,
        BotonOpcion
    },
    data() {
        return {
            eventoSeleccionado: null,
            opcionesEventos: []
        }
    },
    methods: {
        obtenerEventos() {
            this.estaHaciendoPeticion = true
            fetch('http://localhost:5000/events')
                .then(response => response.json())
                .then(data => {
                    this.opcionesEventos = data.data.map((event, index) => {
                        return {
                            value: index,
                            text: event.titulo_evento
                        }
                    })
                })
                .catch(error => {
                    alert('Error al obtener los eventos')
                    console.error(error)
                    this.estaHaciendoPeticion = false
                }),
                this.opcionesEventos = [
                    { value: null, text: 'Seleccione un evento', disabled: true },
                    ...this.opcionesEventos
                ]
            this.estaHaciendoPeticion = false
        },
        borrarEvento() {
            alert('Evento creado')
        },
        limpiarDatos() {
            this.eventoSeleccionado = null
        }
    },
    mounted() {
        // this.obtenerEventos()
    }
}
</script>