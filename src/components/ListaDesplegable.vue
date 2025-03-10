<template>
    <div>
        <!-- Form group with label -->
        <b-form-group :label="valorEtiqueta">
            <!-- Select dropdown with options and state -->
            <b-form-select v-model="selectedOpcion" :options="registroOpciones" :state="listState"></b-form-select>
        </b-form-group>
    </div>
</template>

<script>
/**
 * @component
 * @name ListaDesplegable
 * @description Un componente de lista desplegable con opciones personalizables y validación de estado.
 */
export default {
    name: 'ListaDesplegable',
    props: {
        /**
         * @description Valor inicial de la lista desplegable
         * @default null
         */
        value: {
            default: null
        },
        /**
         * @description Etiqueta del grupo del formulario
         * @type {String}
         * @default "texto"
         */
        etiqueta: {
            type: String,
            default: "texto"
        },
        /**
         * @description Opciones disponibles en la lista desplegable
         * @type {Array}
         * @default [{ value: null, text: 'No hay opciones disponibles', disabled: true }]
         */
        opciones: {
            type: Array,
            default: () => [
                { value: null, text: 'No hay opciones disponibles', disabled: true },
            ]
        }
    },
    data() {
        return {
            /**
             * @description Opción seleccionada en la lista desplegable
             * @type {null|*}
             */
            selectedOpcion: this.value,
            /**
             * @description Valor de la etiqueta del grupo del formulario
             * @type {String}
             */
            valorEtiqueta: this.etiqueta,
            /**
             * @description Registro de opciones disponibles en la lista desplegable
             * @type {Array}
             */
            registroOpciones: this.opciones,
            /**
             * @description Estado de la lista desplegable
             * @type {Boolean|null}
             */
            listState: null
        }
    },
    watch: {
        /**
         * @description Observa cambios en la opción seleccionada
         * @param {*} value - Nueva opción seleccionada
         */
        selectedOpcion(value) {
            this.listState = (value || value == 0) ? true : false;
            this.$emit('input', value);
        },
        /**
         * @description Observa cambios en la prop `value`
         * @param {*} value - Nuevo valor
         */
        value(value) {
            this.selectedOpcion = value;
        },
        /**
         * @description Observa cambios en la prop `opciones`
         * @param {Array} value - Nuevas opciones
         */
        opciones(value) {
            this.registroOpciones = value;
        }
    }
}
</script>