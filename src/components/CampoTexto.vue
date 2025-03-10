<template>
    <div>
        <b-form-group :label="valorEtiqueta">
            <b-form-input v-model="texto" type="text" :placeholder="placeholder" :state="textState"></b-form-input>
        </b-form-group>
    </div>
</template>

<script>
/**
 * @component
 * @name CampoTexto
 * @description Un campo de texto con validación y personalización de etiquetas y marcadores de posición.
 */
export default {
    name: 'CampoTexto',
    props: {
        /**
         * @description Valor inicial del campo de texto.
         * @type {String}
         * @default ""
         */
        value: {
            type: String,
            default: ""
        },
        /**
         * @description Etiqueta del campo de texto.
         * @type {String}
         * @default "texto"
         */
        etiqueta: {
            type: String,
            default: "texto"
        },
        /**
         * @description Placeholder del campo de texto.
         * @type {String}
         * @default ""
         */
        placeholder: {
            type: String,
            default: ""
        }
    },
    data() {
        return {
            /**
             * @description Texto ingresado en el campo de texto.
             * @type { String }
             */
            texto: this.value,
            /**
             * @description Estado de la validación del campo de texto.
             * @type {Boolean|null}
             */
            textState: null,
            /**
             * @description Valor de la etiqueta mostrado.
             * @type {String}
             */
            valorEtiqueta: this.etiqueta,
            /**
             * @description Placeholder mostrado en el campo de texto.
             * @type {String}
             */
            valorPlaceholder: this.placeholder
        }
    },
    computed: {
        /**
         * @description Validador para verificar si el campo de texto no está vacío.
         * @returns {Boolean}
         */
        validador() {
            return this.texto.length > 0;
        }
    },
    watch: {
        /**
         * @description Observa cambios en el texto ingresado.
         * @param {String} value - Nuevo texto ingresado.
         */
        texto(value) {
            this.textState = value ? true : false;
            this.$emit('input', value);
        },
        /**
         * @description Observa cambios en la prop `value`.
         * @param {String} value - Nuevo valor.
         */
        value(value) {
            this.texto = value;
        }
    }
}
</script>

<style scoped>
.custom-input {
    background-color: #f0f0f0;
    /* Color de fondo personalizado */
    color: #333;
    /* Color del texto personalizado */
    border: 1px solid #ccc;
    /* Borde personalizado */
    border-radius: 5px;
    /* Bordes redondeados */
}

.custom-input:focus {
    border-color: #66afe9;
    /* Color del borde cuando está enfocado */
    box-shadow: 0 0 8px rgba(102, 175, 233, 0.6);
    /* Sombra cuando está enfocado */
}
</style>
