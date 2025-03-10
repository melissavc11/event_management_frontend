<template>
    <div class="fondo-carga">
        <b-spinner label="Cargando..."></b-spinner>
    </div>
</template>

<script>
/**
 * @component
 * @name PantallaCarga
 * @description Este componente muestra una pantalla de carga con un spinner.
 */
export default {
    name: 'PantallaCarga'
}
</script>

<style>
.fondo-carga {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(10, 4, 76, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>
