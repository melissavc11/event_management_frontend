<template>
    <div>
        <!-- Alerta con cuenta regresiva para ser ocultada -->
        <b-alert :show="dismissCountDown" dismissible :variant="tipo" @dismissed="dismissCountDown = 0"
            @dismiss-count-down="countDownChanged">
            <p>{{ mensaje }}</p>
            <b-progress :variant="tipo" :max="dismissSecs" :value="dismissCountDown" height="4px"></b-progress>
        </b-alert>
    </div>
</template>

<script>
/**
 * @component
 * @name MensajeAlerta
 * @description Componente de alerta con mensaje personalizable y cuenta regresiva para ocultarse.
 */
export default {
    name: 'MensajeAlerta',
    props: {
        /**
         * @description Mensaje a mostrar en la alerta
         * @type {String}
         * @required
         */
        mensaje: {
            type: String,
            required: true
        },
        /**
         * @description Tipo de alerta (color)
         * @type {String}
         * @default "success"
         */
        tipo: {
            type: String,
            default: "success"
        }
    },
    data() {
        return {
            /**
             * @description Contador regresivo para ocultar la alerta
             * @type {Number}
             */
            dismissCountDown: 0,
            /**
             * @description Número de segundos para ocultar la alerta
             * @type {Number}
             */
            dismissSecs: 10,
            /**
             * @description Indica si la alerta es visible
             * @type {Boolean}
             */
            showDismissibleAlert: false,
        }
    },
    methods: {
        /**
         * @description Método llamado cuando cambia el contador regresivo de la alerta
         * @param {Number} dismissCountDown - Nuevo valor del contador regresivo
         */
        countDownChanged(dismissCountDown) {
            this.dismissCountDown = dismissCountDown
        },
        /**
         * @description Muestra la alerta y reinicia el contador regresivo
         */
        showAlert() {
            this.alertaVisible = true;
            this.dismissCountDown = this.dismissSecs;
        },
        /**
         * @description Oculta la alerta
         */
        hideAlert() {
            this.alertaVisible = false;
        }
    }
}
</script>
