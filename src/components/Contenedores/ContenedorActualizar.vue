<template>
    <div>
        <ListaDesplegable v-model="eventoSeleccionado" :opciones="opcionesEventos" etiqueta="Eventos"/>
        <CampoTexto v-model="tituloEvento" etiqueta="Título del evento"/>
        <SelectorFecha v-model="fechaSeleccionada"/>
        <ListaDesplegable v-model="horaSeleccionada" :opciones="opcionesHorarios" etiqueta="Horarios"/>
        <CampoTexto v-model="descripcionEvento" etiqueta="Descripción del evento"/>
        <ListaDesplegable v-model="ubicacionSeleccionada" :opciones="opcionesUbicaciones" etiqueta="Ubicaciones"/>
        <BotonOpcion @click="actualizarEvento" texto="Actualizar" tipo="aceptar"/>
        <BotonOpcion @click="limpiarDatos" texto="Cancelar" tipo="cancelar"/>
        <PantallaCarga v-if="estaHaciendoPeticion"/>
    </div>
</template>

<script>
import SelectorFecha from '../SelectorFecha.vue'
import ListaDesplegable from '../ListaDesplegable.vue'
import CampoTexto from '../CampoTexto.vue'
import PantallaCarga from '../PantallaCarga.vue'
import BotonOpcion from '../BotonOpcion.vue'
export default {
    name: 'ContenedorActualizar',
    components: {
        SelectorFecha,
        ListaDesplegable,
        CampoTexto,
        PantallaCarga,
        BotonOpcion
    },
    data() {
        return {
            eventoSeleccionado: null,
            fechaSeleccionada: null,
            horaSeleccionada: null,
            fechaHoraEvento: "",
            tituloEvento: null,
            descripcionEvento: null,
            estaHaciendoPeticion: false,
            opcionesEventos: [],
            opcionesHorarios: [
                { value: null, text: 'Seleccione un horario', disabled: true},
                { value: '08:00:00', text: '08:00 - 10:00' },
                { value: '10:00:00', text: '10:00 - 12:00' },
                { value: '12:00:00', text: '12:00 - 14:00' },
                { value: '14:00:00', text: '14:00 - 16:00' },
                { value: '16:00:00', text: '16:00 - 18:00' },
                { value: '18:00:00', text: '18:00 - 20:00' },
                { value: '20:00:00', text: '20:00 - 22:00' },
                { value: '22:00:00', text: '22:00 - 00:00' }
            ],
            opcionesUbicaciones: [],
        }
    },
    watch: {
        fechaSeleccionada(value) {
            this.fechaHoraEvento = value + ' ' + (this.horaSeleccionada? this.horaSeleccionada : "")
        },
        horaSeleccionada(value) {
            this.fechaHoraEvento = (this.fechaSeleccionada? this.fechaSeleccionada : "") + ' ' + value
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
        obtenerUbicaciones() {
            this.estaHaciendoPeticion = true
            fetch('http://localhost:5000/locations')
            .then(response => response.json())
            .then(data => {
                this.opcionesUbicaciones = data.data.map((ubicacion, index) => {
                    return {
                        value: index,
                        text: ubicacion.nombre_ubicacion
                    }
                })
            })
            .catch(error => {
                alert('Error al obtener las ubicaciones')
                console.error(error)
                this.estaHaciendoPeticion = false
            }),
            this.opcionesUbicaciones = [
                { value: null, text: 'Seleccione una ubicación', disabled: true },
                ...this.opcionesUbicaciones
            ]
            this.estaHaciendoPeticion = false
        },
        actualizarEvento() {
            alert('Evento creado')
        },
        limpiarDatos() {
            this.fechaSeleccionada = null
            this.horaSeleccionada = null
            this.fechaHoraEvento = null
            this.tituloEvento = null
            this.descripcionEvento = null
            this.ubicacionSeleccionada = null
        }
    },
    created() {
        // this.obtenerEventos()
        // this.obtenerUbicaciones()
    }
}
</script>