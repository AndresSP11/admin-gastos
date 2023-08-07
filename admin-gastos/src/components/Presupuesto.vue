<script setup>
import { ref } from 'vue'
import Alerta from './Alerta.vue'
const presupuesto=ref(0);
const error=ref();
const emit=defineEmits(['definir-presupuesto'])
const definirPresupuesto=()=>{
    /* En este caso, se va asignar el valor de error, error.value, luego se le va a quitar el valor, para que se deje de presentar el slot */
    if(presupuesto.value<=0){
        error.value="Presupuesto no valido"
        setTimeout(()=>{
            error.value=""
        },3000)
        return
    }

    /* Aqui, vamos asignar el valor de la parte del presupuesto para que cerremos desde el APP una vez enviando
    Estte es siempre y cuando pase el error */
    emit('definir-presupuesto',presupuesto.value)

}

</script> 
<template>
    <form class="presupuesto"
    @submit.prevent="definirPresupuesto">
        <Alerta v-if="error">
            {{ error }}
        </Alerta>
        <div class="campo">
            <label for="nuevo-presupuesto">
                Definir Presupuesto
            </label>
            <input 
            id="nuevo-presupuesto"
            class="nuevo-presupuesto"
            placeholder="Añade tu presupuesto"
            type="number"
            v-model="presupuesto"
            >
        </div>
        <input type="submit" value="Definir Presupuesto">

    </form>

</template>
<style scoped>
    .presupuesto{
        width: 100%;
    }
    .campo{
        display: grid;
        gap:2rem;
        
    }
    .campo label{
        font-size: 2.5rem;
        text-align: center;
        color: var(--azul);
    }
    .presupuesto input[type="number"]{
        background-color: var(--gris-claro); 
        border-radius: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2.2rem;
        width: 100%;
        text-align: center;
    }
    .presupuesto input[type="submit"]{
        background-color: var(--azul);
        border: none;
        padding: 1rem;
        text-align: center;
        font-size: 2rem;
        margin-top: 2rem;
        color: var(--blanco);
        font-weight: 900;
        width: 100%;
        transition: all ease-in-out 300ms;
    }
    .presupuesto input[type="submit"]:hover{
        background-color: rgba(35, 0, 236, 0.918);
        cursor: pointer;
    }
</style>