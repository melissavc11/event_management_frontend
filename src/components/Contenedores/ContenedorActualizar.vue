<template>
    <div>
        <!-- Componente de alerta para mostrar mensajes -->
        <MensajeAlerta :mensaje="mensajeAlerta" :tipo="tipoAlerta" ref="componenteAl" />
        <!-- Lista desplegable para seleccionar el evento a actualizar -->
        <ListaDesplegable v-model="eventoSeleccionado" :opciones="opcionesEventos" etiqueta="Eventos" />
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
            <!-- Botón para actualizar el evento -->
            <BotonOpcion @click="actualizarEvento" texto="Actualizar" tipo="aceptar" />
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
 * @name ContenedorActualizar
 * @description Un contenedor para actualizar un evento seleccionado de una lista desplegable con campos de texto, selecciones de fecha y hora, y listas desplegables.
 */
export default {
    name: 'ContenedorActualizar',
    components: {
        SelectorFecha,
        ListaDesplegable,
        CampoTexto,
        PantallaCarga,
        BotonOpcion,
        MensajeAlerta,
    },
    props: {
        // No hay props definidas para este componente en este momento.
    },
    data() {
        return {
            /**
             * @description Evento seleccionado para actualizar
             * @type {null|*}
             */
            eventoSeleccionado: null,
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
             * @description Descripción del evento
             * @type {String|null}
             */
            descripcionEvento: null,
            /**
             * @description Ubicación seleccionada para el evento
             * @type {String|null}
             */
            ubicacionSeleccionada: null,
            /**
             * @description Opciones disponibles de eventos para seleccionar
             * @type {Array}
             */
            opcionesEventos: [],
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
            tipoAlerta: "",
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
        },
        /**
         * @description Observa cambios en el evento seleccionado
         * @param {String} value - Nuevo evento seleccionado
         */
        eventoSeleccionado(value) {
            this.obtenerEventosId(value)
        },
    },
    methods: {
        /**
         * @description Obtiene los detalles del evento seleccionado por su ID
         * @param {Number} id - ID del evento seleccionado
         */
        obtenerEventosId(id) {
            id = id ? parseInt(id, 10) : null;
            this.estaHaciendoPeticion = true;
            let url = ""
            if (typeof id === 'number') {
                url = `http://localhost:5000/events/${id}`
            }
            else {
                url = 'http://localhost:5000/events'
            }
            fetch(url)
                .then(response => {
                    if (!response.ok) {
                        return response.json().then(errorData => {
                            throw new Error(errorData.error || `HTTP error! status: ${response.status}`);
                        });
                    }
                    return response.json();
                })
                .then(data => {
                    this.tituloEvento = data.data[0].titulo_evento
                    this.fechaHoraEvento = data.data[0].fecha_hora_evento
                    this.fechaSeleccionada = data.data[0].fecha_hora_evento.split(" ")[0]
                    this.horaSeleccionada = data.data[0].fecha_hora_evento.split(" ")[1]
                    this.descripcionEvento = data.data[0].descripcion_evento
                    this.ubicacionSeleccionada = this.opcionesUbicaciones.find(opcion => opcion.text === data.data[0].ubicacion_evento.nombre_ubicacion).value
                    this.estaHaciendoPeticion = false;
                })
                .catch(error => {
                    this.mensajeAlerta = error;
                    this.$refs.componenteAlerta.showAlert()
                    this.estaHaciendoPeticion = false;
                });

        },
        /**
         * @description Obtiene todos los eventos desde el servidor
         */
        obtenerEventos() {
            this.estaHaciendoPeticion = true
            fetch('http://localhost:5000/events')
                .then(response => response.json())
                .then(data => {
                    this.opcionesEventos = data.data.map((event) => {
                        return {
                            value: event.index,
                            text: event.titulo_evento
                        }
                    })
                    let eventoRedirigido = JSON.parse(localStorage.getItem('eventoRedirigido'));
                    if (eventoRedirigido) {
                        this.eventoSeleccionado = eventoRedirigido.index
                        localStorage.removeItem('eventoRedirigido')
                    }
                    this.opcionesEventos = [
                        { value: null, text: 'Seleccione un evento', disabled: true },
                        ...this.opcionesEventos
                    ]
                })
                .catch(error => {
                    alert('Error al obtener los eventos')
                    console.error(error)
                    this.estaHaciendoPeticion = false
                }),
                this.estaHaciendoPeticion = false
        },
        /**
         * @description Obtiene todas las ubicaciones desde el servidor
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
                    this.opcionesUbicaciones = [
                        { value: null, text: 'Seleccione una ubicación', disabled: true },
                        ...this.opcionesUbicaciones
                    ]
                })
                .catch(error => {
                    alert('Error al obtener las ubicaciones')
                    console.error(error)
                    this.estaHaciendoPeticion = false
                }),
                this.estaHaciendoPeticion = false
        },
        /**
         * @description Actualiza el evento seleccionado con los nuevos datos
         */
        actualizarEvento() {
            if (this.tituloEvento == "" || this.fechaHoraEvento != "%Y-%m-%d %H:%M:%S" || this.descripcionEvento == "" || this.ubicacionSeleccionada == null) {
                this.tipoAlerta = "danger";
                this.mensajeAlerta = "Por favor, llene todos los campos.";
                this.$refs.componenteAlerta.showAlert()
                return;
            }
            fetch(`http://localhost:5000/events/${this.eventoSeleccionado}`,
                {
                    method: 'PUT',
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
            this.eventoSeleccionado = null
        },
        /**
         * @description Limpia los datos del formulario
         */
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
        this.obtenerEventos();
        this.obtenerUbicaciones();
    }
}
</script>