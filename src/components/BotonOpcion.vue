<template>
    <div>
        <button @click="emitirEvento" :class="'boton '+estilo" :style="{ fontSize: tamano }">{{ texto }}</button>
    </div>
</template>

<script>
/**
 * @component
 * @name BotonOpcion * @description Un botón que emite un evento al hacer clic y cambia de estilo según el tipo.
 */
export default {
    name: 'BotonOpcion',
    props: {
        /**
         * @description Texto mostrado en el botón.
         * @type {String}
         * @default "texto"
         */
        texto: {
            type: String,
            default: "texto"
        },
        /**
         * @description Tipo de botón que determina su estilo.
         * @type {String}
         * @default "crear"
         */
        tipo: {
            type: String,
            default: "crear"
        },
        /**
         * @description Tamaño del texto en el botón.
         * @type {String}
         * @default "medium"
         */
        tamano: {
            type: String,
            default: "medium"
        }
    },
    data() {
        return {
            /**
             * @description Clases de estilo para cada tipo de botón.
             * @type {Object}
             */
            clasesPorTipo: {
                crear: "crear-boton",
                buscar: "buscar-boton",
                actualizar: "actualizar-boton",
                borrar: "borrar-boton",
                aceptar: "aceptar-boton",
                cancelar: "cancelar-boton"
            }
        }
    },
    computed: {
        /**
         * @description Clase de estilo correspondiente al tipo de botón.
         * @returns {String}
         */
        estilo() {
            return this.clasesPorTipo[this.tipo] || '';
        }
    },
    methods: {
        /**
         * @description Emite un evento de clic.
         * @emits click
         */
        emitirEvento() {
            this.$emit('click');
        }
    }
}
</script>

<style scoped>
/**
 * Estilo base para el botón.
 * - color: color del texto
 * - border: sin borde
 * - border-radius: bordes redondeados
 * - padding: espacio interno
 * - font-size: tamaño del texto
 * - font-weight: peso de la fuente
 * - cursor: tipo de cursor al pasar sobre el botón
 * - box-shadow: sombra del botón
 * - transition: transición de color y transformación
 */
.boton {
    color: white;
    border: none;
    border-radius: 50px;
    padding: 10px 20px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease, transform 0.2s ease;
}

/**
 * Transformación del botón al pasar el cursor.
 */
.boton:hover {
    transform: scale(1.05);
}

/**
 * Transformación del botón al hacer clic.
 */
.boton:active {
    transform: scale(1);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

/**
 * Estilo para el botón de tipo 'crear'.
 */
.crear-boton {
    background-color: #5FE795;
}
.crear-boton:hover, .crear-boton:active {
    background-color: #09B86C;
}

/**
 * Estilo para el botón de tipo 'buscar'.
 */
.buscar-boton {
    background-color: #D2A2F2;
}
.buscar-boton:hover, .buscar-boton:active {
    background-color: #A15CCC;
}

/**
 * Estilo para el botón de tipo 'actualizar'.
 */
.actualizar-boton {
    background-color: #6DBFF2;
}
.actualizar-boton:hover, .actualizaroton:active {
    background-color: #4172CC;
}

/**
 * Estilo para el botón de tipo 'borrar'.
 */
.borrar-boton {
    background-color: #F5B4D4;
}
.borrar-boton:hover, .borrar-boton:active {
    background-color: #DC0063;
}

/**
 * Estilo para el botón de tipo 'aceptar'.
 */
.aceptar-boton {
    background-color: #567694;
}
.aceptar-boton:hover, .aceptar-boton:active {
    background-color: #103087;
}

/**
 * Estilo para el botón de tipo 'cancelar'.
 */
.cancelar-boton {
    background-color: #F08A75;
}
.cancelar-boton:hover, .cancelar-boton:active {
    background-color: #df5565;
}
</style>