<template>
    <div>
        <!-- Componente de alerta para mostrar mensajes -->
        <MensajeAlerta :mensaje="mensajeAlerta" :tipo="tipoAlerta" ref="componenteAlerta" />
        <!-- Campo de texto para el título del evento -->
        <CampoTexto v-model="tituloEvento" etiqueta="Título del evento" placeholder="Escriba el título del evento" />
        <!-- Selector de fecha para la fecha del evento -->
        <SelectorFecha v-model="fechaSeleccionada" />
        <!-- Lista desplegable para seleccionar la hora del evento -->
        <ListaDesplegable v-model="horaSeleccionada" :opciones="opcionesHorarios" etiqueta="Horarios" />
        <!-- Campo de texto para la descripción del evento -->
        <CampoTexto v-model="descripcionEvento" etiqueta="Descripción del evento"
            placeholder="Escriba la descripción del evento" />
        <!-- Lista desplegable para seleccionar la ubicación del evento -->
        <ListaDesplegable v-model="ubicacionSeleccionada" :opciones="opcionesUbicaciones" etiqueta="Ubicaciones" />
        <div class="d-flex justify-content-end">
            <!-- Botón para crear el evento -->
            <BotonOpcion @click="crearEvento" texto="Crear" tipo="aceptar" />
            <!-- Botón para cancelar y limpiar los datos -->
            <BotonOpcion @click="limpiarDatos" texto="Cancelar" tipo="cancelar" />
        </div>
        <!-- Componente de carga para mostrar durante las peticiones -->
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

/**
 * @component
 * @name ContenedorCrear
 * @description Un contenedor para crear un evento con campos de texto, selecciones de fecha y hora, y listas desplegables.
 */
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
            /**
             * @description Título del evento
             * @type {String|null}
             */
            tituloEvento: null,
            /**
             * @description Fecha seleccionada para el evento
             * @type {String|null}
             */
            fechaSeleccionada: null,
            /**
             * @description Hora seleccionada para el evento
             * @type {String|null}
             */
            horaSeleccionada: null,
            /**
             * @description Fecha y hora del evento combinadas
             * @type {String}
             */
            fechaHoraEvento: "",
            /**
             * @description Ubicación seleccionada para el evento
             * @type {String|null}
             */
            ubicacionSeleccionada: null,
            /**
             * @description Opciones disponibles para los horarios
             * @type {Array}
             */
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
            /**
             * @description Opciones disponibles para las ubicaciones
             * @type {Array}
             */
            opcionesUbicaciones: [],
            /**
             * @description Descripción del evento
             * @type {String|null}
             */
            descripcionEvento: null,
            /**
             * @description Indica si se está realizando una petición
             * @type {Boolean}
             */
            estaHaciendoPeticion: false,
            /**
             * @description Mensaje de la alerta
             * @type {String}
             */
            mensajeAlerta: "",
            /**
             * @description Tipo de la alerta
             * @type {String}
             */
            tipoAlerta: ""
        }
    },
    watch: {
        /**
         * @description Observa cambios en la fecha seleccionada
         * @param {String} value - Nueva fecha seleccionada
         */
        fechaSeleccionada(value) {
            this.fechaHoraEvento = value + ' ' + (this.horaSeleccionada ? this.horaSeleccionada : "")
        },
        /**
         * @description Observa cambios en la hora seleccionada
         * @param {String} value - Nueva hora seleccionada
         */
        horaSeleccionada(value) {
            this.fechaHoraEvento = (this.fechaSeleccionada ? this.fechaSeleccionada : "") + ' ' + value
        }
    },
    methods: {
        /**
         * @description Obtiene las ubicaciones desde el servidor
         */
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
        /**
         * @description Crea un evento nuevo y lo envía al servidor
         */
        crearEvento() {
            if (this.tituloEvento == "" || this.fechaSeleccionada == null || this.horaSeleccionada == null || this.descripcionEvento == "" || this.ubicacionSeleccionada == null) {
                this.tipoAlerta = "danger";
                this.mensajeAlerta = "Por favor, llene todos los campos.";
                this.$refs.componenteAlerta.showAlert()
                console.log(this.tituloEvento, this.fechaHoraEvento, this.descripcionEvento, this.ubicacionSeleccionada)
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
        /**
         * @description Limpia todos los datos del formulario
         */
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