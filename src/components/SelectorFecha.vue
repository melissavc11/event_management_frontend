<template>
  <div>
    <!-- Form group para seleccion de fecha -->
    <b-form-group label="Seleccione una fecha">
      <!-- Datepicker componente con validacion y feedback -->
      <b-form-datepicker v-model="selectedDate" :state="dateState" aria-describedby="input-live-feedback"
        :min="fechaMinima" :max="fechaMaxima" placeholder="Seleccione una fecha"></b-form-datepicker>
      <b-form-invalid-feedback id="input-live-feedback">
        Seleccione una fecha válida.
      </b-form-invalid-feedback>
    </b-form-group>
  </div>
</template>

<script>
/**
 * @component
 * @name SelectorFecha
 * @description Un componente de selector de fecha con validación y rango de fechas personalizable.
 */
export default {
  name: 'SelectorFecha',
  props: {
    /**
     * @description Valor inicial del selector de fecha
     * @type {String}
     * @default null
     */
    value: {
      type: String,
      default: null
    }
  },
  data() {
    return {
      /**
       * @description Fecha seleccionada
       * @type {String|null}
 */
      selectedDate: this.value,
      /**
       * @description Estado de la validación de la fecha
       * @type {Boolean|null}
       */
      dateState: null,
      /**
       * @description Fecha mínima seleccionable
       * @type {Date}
       */
      fechaMinima: new Date(),
      /**
       * @description Fecha máxima seleccionable
       * @type {}
       */
      fechaMaxima: new Date(new Date().setFullYear(new Date().getFullYear() + 2))
    }
  },
  watch: {
    /**
     * @description Observa cambios en la fecha seleccionada
     * @param {String} value - Nueva fecha seleccionada
     */
    selectedDate(value) {
      this.dateState = value ? true : false;
      this.$emit('input', value);
    },
    /**
     * @description Observa cambios en la prop `value`
     * @param {} value - Nuevo valor
     */
    value(value) {
      this.selectedDate = value;
    }
  }
}
</script>