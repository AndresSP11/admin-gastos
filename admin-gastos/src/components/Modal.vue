<script setup>
    /* Este importe de cerrar modal sera para la imagenes */
    import cerrarModal from '../assets/img/cerrar.svg'
    import Alerta from './Alerta.vue'
    import { ref } from 'vue'

    
    const emit=defineEmits(['cerrar-modal','update:nombre','update:cantidad','update:categoria','guardar-gasto'])
    const error=ref('')
    
    const props=defineProps({
        modal:{
            type:Object,
            required:true
        },
        nombre:{
            type:String,
            required:true
        },
        cantidad:{
            type:[String,Number],
            required:true
        },
        categoria:{
            type:String,
            required:true
        }
    })
    const agregarGasto=()=>{
        /* Validar que todos los campso tengan algo */
        const {nombre,cantidad,categoria}=props
        if([nombre,cantidad,categoria].includes('')){
            error.value='Todo los campos son obligatorios'
            setTimeout(()=>{
                error.value=''
            },3000)
            
            console.log('Hay campos vacios')
            return
        }
        if(cantidad<=0){
            error.value='Cantidad no valida'
            setTimeout(()=>{
                error.value='' 
            },3000) 
            return
        }

        emit('guardar-gasto');
    }
</script>
<template>
    <div class="modal">
        <!-- Aqui es para el cerrar Modal  -->
        <img :src="cerrarModal" alt="" 
        @click="$emit('cerrar-modal')">
        <div class="contenedor contenedor-formulario"
        :class="[modal.animar ? 'animar' : 'cerrar']">
            <form class="nuevo-gasto"
            @submit.prevent="agregarGasto"
            >
                <Alerta v-if="error">
                {{ error }}</Alerta>
                <legend>Añadir Gasto</legend>
                <div class="campo">
                    <label for="">Nombre Gasto:</label>
                    <input 
                    type="text"
                    id="nombre"
                    placeholder="Añade el Nombre del Gasto, ej. 300"
                    :value="nombre"
                    @input="$emit('update:nombre',$event.target.value)"
                    >
                </div>
                <div class="campo">
                    <label for="">Cantidad Dinero:</label>
                    <input 
                    type="Number"
                    id="nombre"
                    placeholder="Añade la cantidad del Gasto, ej. 300"
                    :value="cantidad"
                    @input="$emit('update:cantidad',+$event.target.value)"
                    >
                </div>
                <div class="campo">
                    <label for="nombre">Categoria:</label>
                    <select
                    :value="categoria"
                    @input="$emit('update:categoria',$event.target.value)">
                        <option value="">--Seleccione--</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripciones</option>
                    </select>
                </div>

                <input type="submit"
                value="Añadir Gasto">
            </form>
        </div>
    </div>
</template>
<style scoped>
    .contenedor{
        background-color: rgba(12, 1, 1, 0.89);
    }
    .contenedor-formulario{
        transition: all ease-in-out 300ms;
        opacity: 0;
    }
    .contenedor-formulario.animar{
        opacity: 1;
    }
    .contenedor-formulario.cerrar{
        opacity: 0;
    }
    .modal{
        position: absolute;
        background-color: rgba(12, 1, 1, 0.89);
        top: 0;
        left: 0px;
        right: 0;
        bottom: 0;
    }
    .modal img{
        width: 5rem;
        position: absolute;
        right: 4rem;
        top: 4rem;
        cursor: pointer;
    }
    .campo{
        display: grid;
        gap: 2rem;
    }
    .nuevo-gasto{
        margin: 10rem auto 0 auto;
    
    }
    .nuevo-gasto legend{
        color: var(--blanco);
        font-size: 3rem;
        margin: 0 auto;
        font-weight: 800;
        margin-bottom: 3rem;
    }
    .nuevo-gasto input, 
    .nuevo-gasto select{
        background-color: var(--gris-claro);
        border-radius: 1rem;
        margin-bottom: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2.2rem;
    }
    .nuevo-gasto label{
        color: var(--blanco);
        font-size: 3rem;
    }
    .nuevo-gasto input[type="submit"]{
        background-color: var(--azul);
        width: 100%;
        cursor: pointer;
        font-weight: 700 ;
    }
</style>