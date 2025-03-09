<template>
    <div>
        <b-form-group :label="valorEtiqueta">
            <b-form-select v-model="selectedTime" :options="timeOptions"></b-form-select>
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
            selectedTime: this.value,
            valorEtiqueta: this.etiqueta,
            timeOptions : this.opciones,
        }
    },
    watch: {
        selectedTime(value) {
            this.$emit('input', value);
        },
        value(value) {
            this.selectedTime = value;
        }
    }
}
</script>