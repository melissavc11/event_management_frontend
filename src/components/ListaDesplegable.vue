<template>
    <div>
        <b-form-group :label="valorEtiqueta">
            <b-form-select v-model="selectedOpcion" :options="registroOpciones"></b-form-select>
        </b-form-group>
    </div>
</template>

<script>
export default {
    props: {
        value: {
            default: null
        },
        etiqueta: {
            type: String,
            default: "texto"
        },
        opciones: {
            type: Array,
            default: () => [
                { value: null, text: 'No hay opciones disponibles', disabled: true },
            ]
        }
    },
    data() {
        return {
            selectedOpcion: this.value,
            valorEtiqueta: this.etiqueta,
            registroOpciones : this.opciones,
        }
    },
    watch: {
        selectedOpcion(value) {
            this.$emit('input', value);
        },
        value(value) {
            this.selectedOpcion = value;
        },
        opciones(value) {
            this.registroOpciones = value;
        }
    }
}
</script>