<template>
    <div>
        <MensajeAlerta :mensaje="mensajeAlerta" :tipo="tipoAlerta" ref="componenteAlerta" />
        <CampoTexto v-model="tituloEvento" etiqueta="Título del evento" placeholder="Escriba el título del evento"/>
        <SelectorFecha v-model="fechaSeleccionada" />
        <ListaDesplegable v-model="horaSeleccionada" :opciones="opcionesHorarios" etiqueta="Horarios" />
        <CampoTexto v-model="descripcionEvento" etiqueta="Descripción del evento" placeholder="Escriba la descripción del evento"/>
        <ListaDesplegable v-model="ubicacionSeleccionada" :opciones="opcionesUbicaciones" etiqueta="Ubicaciones" />
        <div class="d-flex justify-content-end">
            <BotonOpcion @click="crearEvento" texto="Crear" tipo="aceptar" />
            <BotonOpcion @click="limpiarDatos" texto="Cancelar" tipo="cancelar" />
        </div>
        <PantallaCarga v-if="estaHaciendoPeticion" />
    </div>
</template>

<script>
import SelectorFecha from '../SelectorFecha.vue'
import ListaDesplegable from '../ListaDesplegable.vue'
import CampoTexto from '../CampoTexto.vue'
import PantallaCarga from '../PantallaCarga.vue'
import BotonOpcion from '../BotonOpcion.vue'
import MensajeAlerta from '../MensajeAlerta.vue'
export default {
    name: 'ContenedorCrear',
    components: {
        SelectorFecha,
        ListaDesplegable,
        CampoTexto,
        PantallaCarga,
        BotonOpcion,
        MensajeAlerta
    },
    data() {
        return {
            tituloEvento: null,
            fechaSeleccionada: null,
            horaSeleccionada: null,
            fechaHoraEvento: "",
            ubicacionSeleccionada: null,
            opcionesHorarios: [
                { value: null, text: 'Seleccione un horario', disabled: true },
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
            descripcionEvento: null,
            estaHaciendoPeticion: false,
            mensajeAlerta: "",  
            tipoAlerta: ""
        }
    },
    watch: {
        fechaSeleccionada(value) {
            this.fechaHoraEvento = value + ' ' + (this.horaSeleccionada ? this.horaSeleccionada : "")
        },
        horaSeleccionada(value) {
            this.fechaHoraEvento = (this.fechaSeleccionada ? this.fechaSeleccionada : "") + ' ' + value
        }
    },
    methods: {
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
                    this.estaHaciendoPeticion = false
                    this.opcionesUbicaciones = [
                        { value: null, text: 'Seleccione una ubicación', disabled: true },
                        ...this.opcionesUbicaciones
                    ]
                })
                .catch(error => {
                    alert('Error al obtener las ubicaciones')
                    console.error(error)
                    this.estaHaciendoPeticion = false
                })
        },
        crearEvento() {
            if (this.tituloEvento == "" || this.fechaHoraEvento != "%Y-%m-%d %H:%M:%S" || this.descripcionEvento == "" || this.ubicacionSeleccionada == null) {
                this.tipoAlerta = "danger";
                this.mensajeAlerta = "Por favor, llene todos los campos.";
                this.$refs.componenteAlerta.showAlert()
                return;
            }
            fetch('http://localhost:5000/events',
                {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        titulo_evento: this.tituloEvento,
                        fecha_hora_evento: this.fechaHoraEvento,
                        descripcion_evento: this.descripcionEvento,
                        ubicacion_evento: this.ubicacionSeleccionada
                    })
                })
                .then(response => {
                    if (!response.ok) {
                        return response.json().then(errorData => {
                            throw new Error(errorData.error || `HTTP error! status: ${response.status}`);
                        });
                    }
                    return response.json();
                })
                .then(data => {
                    this.tipoAlerta = "success";
                    this.mensajeAlerta = data.mensaje;
                    this.$refs.componenteAlerta.showAlert()
                    this.limpiarDatos()
                })
                .catch(error => {
                    this.tipoAlerta = "danger";
                    this.mensajeAlerta = error;
                    this.$refs.componenteAlerta.showAlert()
                    console.error(error)
                })
        },
        limpiarDatos() {
            this.tituloEvento = null
            this.fechaSeleccionada = null
            this.horaSeleccionada = null
            this.fechaHoraEvento = null
            this.descripcionEvento = null
            this.ubicacionSeleccionada = null
        }
    },
    created() {
        this.obtenerUbicaciones()
    }
}
</script>