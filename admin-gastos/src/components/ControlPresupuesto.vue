<script setup>
    import { computed } from 'vue'
    import CircleProgress from 'vue3-circle-progress'
    import "vue3-circle-progress/dist/circle-progress.css"
    import { formatearCantidad } from '../assets/helpers';
    defineEmits(['reset-app'])
    const props=defineProps({
        presupuesto:{
            type:Number,
            required:true
        },
        disponible:{
            type:Number,
            required:true
        },
        gastado:{
            type:Number,
            required:true
        }
    })
    const porcentaje=computed(()=>{
        return parseInt(((props.presupuesto - props. disponible)/props.presupuesto)*100)
    })
</script>

<template>
    <div class="dos-columnas">
        <div class="contenedor-grafico">
            <p class="porcentaje">{{ porcentaje }}%</p>
            <!-- AQUI VAMOS A EMITIR LA FUNCIO DE PORCANTJE -->
            <CircleProgress
            :percent="porcentaje"
            :size="250"
            :border-width="30"
            :border-bg-width="30"
            fill-color="#3b82f6"
            empty-color="#F5F5F5"></CircleProgress>
        </div>
        <div class="contenedor-presupuesto">
            <button class="reset-app"
            @click="$emit('reset-app')">Resetear App</button>
            <p>
                <span>Presupuesto</span>
                <!-- Formartear cantidad, le esta cubriendo desde JavaScript  -->
                {{ formatearCantidad(presupuesto) }}
            </p>
            <p>
                <!-- Formartear cantidad, le esta cubriendo desde JavaScript  -->
                <span>Disponible</span>
                {{ formatearCantidad(disponible) }}
            </p>
            <p>
                <span>Gastado:</span>
                {{ formatearCantidad(gastado) }}
            </p>
        </div>
    </div>
</template>

<style scoped>
    .dos-columnas{
        display: flex;
        flex-direction: column;
    }
    .dos-columnas :first-child{
        margin-bottom: 20px;
    }
    @media (min-width: 768px){
        .dos-columnas{
            flex-direction: row;
            gap: 4rem;
            align-items: center;
        }
    }
    .reset-app{
        font-weight: 900;
        text-align: center;
        width: 100%;
        padding: 1rem;
        color: var(--blanco);
        text-transform: uppercase;
        background-color: rgb(202, 34, 169);
        border-radius: 5px;
        transition: ease-in-out .3s all;
        border: none;
    }
    .reset-app:hover{
        background-color: rgb(170, 37, 155);
        cursor:pointer;
    }
    .contenedor-presupuesto{
        width: 100%;
    }
    .contenedor-presupuesto p{
        font-size: 2.4rem;
        text-align: center;
    }
    @media (min-width: 768px){
        .contenedor-presupuesto p{
        font-size: 2.4rem;
        text-align: left;
    }   
    }
    .contenedor-presupuesto span{
        font-weight: 900;
        color: var(--azul);
    }
    .contenedor-grafico{
        position:relative;
    }
    .porcentaje{
        position: absolute;
        margin: auto;
        top: calc(50%-1.5rem);
        left: 0;
        right: 0;
        text-align: center;
        z-index: 100;
        font-size: 3rem;
        margin-top: 100px;
        font-weight: 900;
        color: var(--azul);
    }
</style>