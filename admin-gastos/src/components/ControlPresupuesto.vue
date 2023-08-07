<script setup>
    import imagen from '../assets/img/grafico.jpg'
    import { formatearCantidad } from '../assets/helpers';
    const props=defineProps({
        presupuesto:{
            type:Number,
            required:true
        },
        disponible:{
            type:Number,
            required:true
        }
    })
</script>

<template>
    <div class="dos-columnas">
        <div class="contenedor-grafico">
            <img :src="imagen">
        </div>
        <div class="contenedor-presupuesto">
            <button class="reset-app">Resetear App</button>
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
                $0
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
</style>