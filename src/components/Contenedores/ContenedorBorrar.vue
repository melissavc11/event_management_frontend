<template>
    <div>
        <!-- Componente de alerta para mostrar mensajes -->
        <MensajeAlerta :mensaje="mensajeAlerta" :tipo="tipoAlerta" ref="componenteAlerta" />
        <!-- Lista desplegable para seleccionar el evento a borrar -->
        <ListaDesplegable v-model="eventoSeleccionado" :opciones="opcionesEventos" etiqueta="Eventos" />
        <div class="d-flex justify-content-end">
            <!-- Botón para borrar el evento seleccionado -->
            <BotonOpcion @click="borrarEvento" texto="Borrar" tipo="aceptar" />
            <!-- Botón para cancelar y limpiar los datos -->
            <BotonOpcion @click="limpiarDatos" texto="Cancelar" tipo="cancelar" />
        </div>
    </div>
</template>

<script>
import ListaDesplegable from '../ListaDesplegable.vue';
import BotonOpcion from '../BotonOpcion.vue';
import MensajeAlerta from '../MensajeAlerta.vue';

/**
 * @component
 * @name ContenedorBorrar
 * @description Un contenedor para borrar un evento seleccionado de una lista desplegable.
 */
export default {
    name: 'ContenedorBorrar',
    components: {
        ListaDesplegable,
        BotonOpcion,
        MensajeAlerta
    },
    props: {
        /**
         * @description Evento pasado como prop
         * @default null
         */
        evento: {
            default: null
        }
    },
    data() {
        return {
            /**
             * @description Evento seleccionado para borrar
             * @type {null|*}
             */
            eventoSeleccionado: null,
            /**
             * @description Opciones disponibles de eventos para seleccionar
             * @type {Array}
             */
            opcionesEventos: [],
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
    methods: {
        /**
         * @description Obtiene los eventos desde el servidor
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
         * @description Borra el evento seleccionado
         */
        borrarEvento() {
            if (this.eventoSeleccionado == null) {
                this.tipoAlerta = "danger";
                this.mensajeAlerta = "Por favor, seleccione un evento.";
                this.$refs.componenteAlerta.showAlert()
                return;
            }
            fetch(`http://localhost:5000/events/${this.eventoSeleccionado}`,
                {
                    method: 'DELETE',
                    headers: {
                        'Content-Type': 'application/json'
                    }

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
         * @description Limpia los datos del formulario
         */
        limpiarDatos() {
            this.eventoSeleccionado = null
        }
    },
    watch: {
        /**
         * @description Observa cambios en la prop `evento`
         * @param {*} value - Nuevo valor de la prop `evento`
         */
        evento(value) {
            this.eventoSeleccionado = this.opcionesEventos.find(opcion => opcion.value === value.index).value
        }
    },
    mounted() {
        this.obtenerEventos();
    }
}
</script>