<template>
    <div>
        <b-form-group :label="valorEtiqueta">
            <b-form-input v-model="texto" type="text" :placeholder="placeholder" :state="textState"></b-form-input>
        </b-form-group>
    </div>
</template>

<script>
export default {
    props: {
        value: {
            type: String,
            default: ""
        },
        etiqueta: {
            type: String,
            default: "texto"
        },
        placeholder: {
            type: String,
            default: ""
        }
    },
    computed: {
        validador() {
            return this.texto.length > 0;
        }
    },
    data() {
        return {
            texto: this.value,
            textState: null,
            valorEtiqueta: this.etiqueta,
            valorPlaceholder: this.placeholder
        }
    },
    watch: {
        texto(value) {
            this.textState = value?  true : false;
            this.$emit('input', value);
        },
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