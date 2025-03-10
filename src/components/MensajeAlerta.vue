<template>
    <div>
        <b-alert :show="dismissCountDown" dismissible :variant="tipo" @dismissed="dismissCountDown = 0"
            @dismiss-count-down="countDownChanged">
            <p>{{ mensaje }}</p>
            <b-progress :variant="tipo" :max="dismissSecs" :value="dismissCountDown" height="4px"></b-progress>
        </b-alert>
    </div>
</template>

<script>
export default {
    name: 'MensajeAlerta',
    props: {
        mensaje: {
            type: String,
            required: true
        },
        tipo: {
            type: String,
            default: "success"
        }
    },
    data() {
        return {
            dismissCountDown: 0,
            dismissSecs: 10,
            showDismissibleAlert: false,
        }

    },
    methods: {
        countDownChanged(dismissCountDown) {
            this.dismissCountDown = dismissCountDown
        },
        showAlert() {
            this.alertaVisible = true;
            this.dismissCountDown = this.dismissSecs;
        },
        hideAlert() {
            this.alertaVisible = false;
        }
    }

}
</script>