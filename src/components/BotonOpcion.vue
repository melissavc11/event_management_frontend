<template>
    <div>
        <button @click="emitirEvento" :class="'boton '+estilo" :style="{ fontSize: tamano }">{{ texto }}
        </button>
    </div>
</template>

<script>
export default {
    props: {
        texto: {
            type: String,
            default: "texto"
        },
        tipo: {
            type: String,
            default: "crear"
        },
        tamano: {
            type: String,
            default: "medium"
        }
    },
    computed: {
        estilo() {
            return this.clasesPorTipo[this.tipo] || '';
        }
    },
    data() {
        return {
            clasesPorTipo: {
                crear: "crear-boton",
                buscar: "buscar-boton",
                actualizar: "actualizar-boton",
                borrar: "borrar-boton",
                aceptar: "aceptar-boton",
                cancelar: "cancelar-boton"
            }
        }
    },
    methods: {
        emitirEvento() {
            this.$emit('click');
        }
    }
}
</script>

<style scoped>
.boton {
    color: white; 
    border: none; 
    border-radius: 50px; 
    padding: 10px 20px; 
    font-size: 16px; 
    font-weight: bold; 
    cursor: pointer; 
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); 
    transition: background-color 0.3s ease, transform 0.2s ease; 
}
.boton:hover {
    transform: scale(1.05); 
}
.boton:active {
    transform: scale(1); 
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); 
}

.crear-boton {
    background-color: #5FE795;
}
.crear-boton:hover, .crear-boton:active {
    background-color: #09B86C;
}

.buscar-boton {
    background-color: #D2A2F2;
}
.buscar-boton:hover, .buscar-boton:active {
    background-color: #A15CCC;
}

.actualizar-boton {
    background-color: #6DBFF2;
}
.actualizar-boton:hover, .actualizar-boton:active {
    background-color: #4172CC;
}

.borrar-boton {
    background-color: #F5B4D4;
}
.borrar-boton:hover, .borrar-boton:active {
    background-color: #DC0063;
}

.aceptar-boton {
  background-color: #567694;
}
.aceptar-boton:hover, .aceptar-boton:active {
  background-color: #103087;
}

.cancelar-boton {
  background-color: #F08A75;
}
.cancelar-boton:hover, .cancelar-boton:active {
  background-color: #df5565;
}
</style>
