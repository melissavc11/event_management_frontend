<template>
    <div>
        <!-- Componente de alerta para mostrar mensajes -->
        <MensajeAlerta :mensaje="mensajeAlerta" tipo="danger" ref="componenteAlerta" />
        <!-- Input para buscar eventos por índice -->
        <b-form-input v-model="indexEvento" type="number" placeholder="Buscar por index" :min="1"
            :formatter="formateador"></b-form-input>
        <div class="d-flex justify-content-end py-4">
            <!-- Botón para buscar eventos -->
            <BotonOpcion @click="obtenerEventos(indexEvento)" texto="Buscar" tipo="aceptar" />
        </div>
        <!-- Tabla para mostrar los eventos -->
        <b-table :items="items" :fields="fields" responsive="sm">
            <template #cell(options)="data">
                <!-- Botón para actualizar un evento -->
                <BotonOpcion @click="actualizarEvento(data.item)" texto="Actualizar" tipo="actualizar" tamano="small"
                    style="margin-bottom: 5%;" />
                <!-- Botón para borrar un evento -->
                <BotonOpcion @click="borrarEvento(data.item)" texto="Borrar" tipo="borrar" tamano="small" />
            </template>
        </b-table>
        <!-- Componente de carga para mostrar durante las peticiones -->
        <PantallaCarga v-if="estaHaciendoPeticion" />
    </div>
</template>

<script>
import BotonOpcion from '../BotonOpcion.vue'
import PantallaCarga from '../PantallaCarga.vue'
import MensajeAlerta from '../MensajeAlerta.vue'

/**
 * @component
 * @name ContenedorBuscar
 * @description Un contenedor para buscar eventos, con funcionalidades para actualizar y borrar eventos.
 */
export default {
    name: 'ContenedorBuscar',
    components: {
        BotonOpcion,
        PantallaCarga,
        MensajeAlerta
    },
    data() {
        return {
            /**
             * @description Indica si se está realizando una petición
             * @type {Boolean}
             */
            estaHaciendoPeticion: false,
            /**
             * @description Índice del evento a buscar
             * @type {Number|null}
             */
            indexEvento: null,
            /**
             * @description Campos de la tabla de eventos
             * @type {Array}
             */
            fields: [
                { key: 'index', label: '#' },
                { key: 'titulo_evento', label: 'Titulo', sortable: true },
                { key: 'fecha_hora_evento', label: 'Fecha', sortable: true },
                { key: 'descripcion_evento', label: 'Descripción', sortable: true },
                { key: 'ubicacion_evento', label: 'Ubicacion', sortable: true },
                { key: 'options', label: 'Opciones', sortable: false }
            ],
            /**
             * @description Lista de eventos
             * @type {Array}
             */
            items: [],
            /**
             * @description Mensaje de la alerta
             * @type {String}
             */
            mensajeAlerta: ""
        }
    },
    methods: {
        /**
         * @description Formatea el valor del input para que solo contenga números
         * @param {String} value - Valor del input
         * @returns {String} Valor formateado
         */
        formateador(value) {
            let numeros = value.match(/[0-9]+/g);
            return numeros ? numeros.join('') : '';
        },
        /**
         * @description Obtiene los eventos desde el servidor
         * @param {Number|null} id - ID del evento a buscar (opcional)
         */
        obtenerEventos(id) {
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
                    this.items = data.data.map((event) => {
                        return {
                            index: event.index,
                            titulo_evento: event.titulo_evento,
                            fecha_hora_evento: event.fecha_hora_evento,
                            descripcion_evento: event.descripcion_evento,
                            ubicacion_evento: event.ubicacion_evento.nombre_ubicacion + " - " + event.ubicacion_evento.direccion_ubicacion
                        };
                    });
                    this.estaHaciendoPeticion = false;
                })
                .catch(error => {
                    this.mensajeAlerta = error;
                    this.$refs.componenteAlerta.showAlert()
                    this.estaHaciendoPeticion = false;
                });

        },
        /**
         * @description Redirige al componente de actualización de eventos con el evento seleccionado
         * @param {Object} evento - Evento seleccionado
         */
        actualizarEvento(evento) {
            localStorage.setItem('eventoRedirigido', JSON.stringify(evento));
            this.$emit('redirigirActualizarEvento', evento);
        },
        /**
         * @description Redirige al componente de borrado de eventos con el evento seleccionado
         * @param {Object} evento - Evento seleccionado
         */
        borrarEvento(evento) {
            localStorage.setItem('eventoRedirigido', JSON.stringify(evento));
            this.$emit('redirigirBorrarEvento', evento);
        }
    },
    mounted() {
        this.obtenerEventos();
    },
}
</script>