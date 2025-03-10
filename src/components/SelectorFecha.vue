<template>
  <div>
    <b-form-group label="Seleccione una fecha">
      <b-form-datepicker v-model="selectedDate" :state="dateState" aria-describedby="input-live-feedback"
        :min="fechaMinima" :max="fechaMaxima" placeholder="Seleccione una fecha"></b-form-datepicker>
      <b-form-invalid-feedback id="input-live-feedback">
        Seleccione una fecha válida.
      </b-form-invalid-feedback>
    </b-form-group>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: null
    }
  },
  data() {
    return {
      selectedDate: this.value,
      dateState: null,
      fechaMinima: new Date(),
      fechaMaxima: new Date(new Date().setFullYear(new Date().getFullYear() + 2))
    }
  },
  watch: {
    selectedDate(value) {
      this.dateState = value ? true : false;
      this.$emit('input', value);
    },
    value(value) {
      this.selectedDate = value;
    }
  }
}
</script>