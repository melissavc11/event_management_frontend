<template>
    <div>
        <MensajeAlerta :mensaje="mensajeAlerta" tipo="danger" ref="componenteAlerta" />
        <b-form-input v-model="indexEvento" type="number" placeholder="Buscar por index" :min="1" :formatter="formateador"></b-form-input>
        <div class="d-flex justify-content-end py-4">
            <BotonOpcion @click="obtenerEventos(indexEvento)" texto="Buscar" tipo="aceptar" />
        </div>
        <b-table :items="items" :fields="fields" responsive="sm">
            <template #cell(options)="data">
                <BotonOpcion @click="actualizarEvento(data.item)" texto="Actualizar" tipo="actualizar" tamano="small" style="margin-bottom: 5%;"/>
                <BotonOpcion @click="borrarEvento(data.item)" texto="Borrar" tipo="borrar" tamano="small"/>
            </template>
        </b-table>
        <PantallaCarga v-if="estaHaciendoPeticion" />
    </div>
</template>

<script>
import BotonOpcion from '../BotonOpcion.vue'
import PantallaCarga from '../PantallaCarga.vue'
import MensajeAlerta from '../MensajeAlerta.vue'

export default {
    name: 'ContenedorBuscar',
    components: {
        BotonOpcion,
        PantallaCarga,
        MensajeAlerta
    },
    data() {
        return {
            estaHaciendoPeticion: false,
            indexEvento: null,
            fields: [
                { key: 'index', label: '#' },
                { key: 'titulo_evento', label: 'Titulo', sortable: true },
                { key: 'fecha_hora_evento', label: 'Fecha', sortable: true },
                { key: 'descripcion_evento', label: 'Descripción', sortable: true },
                { key: 'ubicacion_evento', label: 'Ubicacion', sortable: true },
                { key: 'options', label: 'Opciones', sortable: false }
            ],
            items: [],
            mensajeAlerta: ""
        }
    },
    methods: {
        formateador(value){
            let numeros = value.match(/[0-9]+/g);
            return numeros ? numeros.join('') : '';
        },
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
        actualizarEvento(evento) {
            console.log(evento)
            localStorage.setItem('eventoRedirigido', JSON.stringify(evento));
            this.$emit('redirigirActualizarEvento', evento);
        },
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