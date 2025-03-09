<template>
    <div>
        <b-form-input v-model="indexEvento" type="number" placeholder="Buscar por index" :min="1"></b-form-input>
        <BotonOpcion @click="obtenerEventos(indexEvento)" texto="Buscar" tipo="aceptar"/>
        <b-table :items="items" :fields="fields" :sort-by.sync="sortBy" :sort-desc.sync="sortDesc" responsive="sm">
            <template #cell(index)="data">
                {{ data.index + 1 }}
            </template>
            <template #cell(options)="data">
                <b-button @click="actualizar_evento(data.item)" variant="primary" title="Actualizar evento">
                    <span class="material-symbols-outlined">
                        edit
                    </span>
                </b-button>
                <b-button @click="borrar_evento(data.item)" variant="danger" title="Borrar evento">
                    <span class="material-symbols-outlined">
                        delete
                    </span>
                </b-button>
            </template>
        </b-table>
        <PantallaCarga v-if="estaHaciendoPeticion"/>
    </div>
</template>

<script>
import BotonOpcion from '../BotonOpcion.vue'
import PantallaCarga from '../PantallaCarga.vue'

export default {
    name: 'ContenedorBuscar',
    components: {
        BotonOpcion,
        PantallaCarga
    },
    data() {
        return {
            estaHaciendoPeticion: false,
            indexEvento: null,
            fields: [
                { key: 'index', label: '#' },
                { key: 'titulo_evento', label: 'Titulo' },
                { key: 'fecha_hora_evento', label: 'Fecha' },
                { key: 'descripcion_evento', label: 'Descripción' },
                { key: 'ubicacion_evento', label: 'Ubicacion' },
                { key: 'options', label: 'Opciones' }
            ],
            items: []
        }
    },
    methods: {
        obtenerEventos(id) {
            id = parseInt(id, 10)
            this.estaHaciendoPeticion = true;
            let url =  ""
            console.log(typeof id)
            if (typeof id === 'number'){
                url = `http://localhost:5000/events/${id}`
            }
            else {
                url = 'http://localhost:5000/events'
            }
            fetch(url)
                .then(response => response.json())
                .then(data => {
                    this.items = data.data.map((event, index) => {
                        return {
                            index: index,
                            titulo_evento: event.titulo_evento,
                            fecha_hora_evento: event.fecha_hora_evento,
                            descripcion_evento: event.descripcion_evento,
                            ubicacion_evento: event.ubicacion_evento
                        }
                    });
                    this.estaHaciendoPeticion = false;
                })
                .catch(error => {
                    alert('Error al obtener los eventos');
                    console.error(error);
                    this.estaHaciendoPeticion = false;
                });
        }
    },
    mounted() {
        this.obtenerEventos();
    },
}
</script>
