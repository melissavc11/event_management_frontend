<template>
    <div>
        <MensajeAlerta :mensaje="mensajeAlerta" :tipo="tipoAlerta" ref="componenteAlerta" />
        <ListaDesplegable v-model="eventoSeleccionado" :opciones="opcionesEventos" etiqueta="Eventos" />
        <CampoTexto v-model="tituloEvento" etiqueta="Título del evento" placeholder="Escriba el título del evento"/>
        <SelectorFecha v-model="fechaSeleccionada" />
        <ListaDesplegable v-model="horaSeleccionada" :opciones="opcionesHorarios" etiqueta="Horarios" />
        <CampoTexto v-model="descripcionEvento" etiqueta="Descripción del evento" placeholder="Escriba la descripción del evento"/>
        <ListaDesplegable v-model="ubicacionSeleccionada" :opciones="opcionesUbicaciones" etiqueta="Ubicaciones" />
        <div class="d-flex justify-content-end">
            <BotonOpcion @click="actualizarEvento" texto="Actualizar" tipo="aceptar" />
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
        
    },
    data() {
        return {
            eventoSeleccionado: null,
            tituloEvento: null,
            fechaSeleccionada: null,
            horaSeleccionada: null,
            fechaHoraEvento: "",
            descripcionEvento: null,
            ubicacionSeleccionada: null,
            opcionesEventos: [],
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
            estaHaciendoPeticion: false,
            mensajeAlerta: "",
            tipoAlerta: "",
        }
    },
    watch: {
        fechaSeleccionada(value) {
            this.fechaHoraEvento = value + ' ' + (this.horaSeleccionada ? this.horaSeleccionada : "")
        },
        horaSeleccionada(value) {
            this.fechaHoraEvento = (this.fechaSeleccionada ? this.fechaSeleccionada : "") + ' ' + value
        },
        eventoSeleccionado(value) {
            this.obtenerEventosId(value)
        },
    },
    methods: {
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
        this.obtenerEventos()
        this.obtenerUbicaciones()
    }
}
</script>