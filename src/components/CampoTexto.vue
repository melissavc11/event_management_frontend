<template>
    <div>
        <b-form-group :label="valorEtiqueta">
            <b-form-input v-model="texto" type="text"></b-form-input>
        </b-form-group>
    </div>
</template>

<script>
export default {
    props: {
        value: {
            type: String,
            default: null
        },
        etiqueta: {
            type: String,
            default: "texto"
        }
    },
    data() {
        return {
            texto: this.value,
            valorEtiqueta: this.etiqueta
        }
    },
    watch: {
        texto(value) {
            this.$emit('input', value);
        },
        value(value) {
            this.texto = value;
        }
    }
}
</script>