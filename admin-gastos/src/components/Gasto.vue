<script setup>
import { formatearCantidad } from '../assets/helpers';

const props=defineProps({
    gasto:{
        type:Object,
        required:true
    }
})
</script>

<template>
    <div class="gasto sombre">
        <div class="contenido">
            <div class="detalles">

            </div>
        </div>
        <p>{{ formatearCantidad(gasto.cantidad) }}</p>
    </div>

</template>

<style>

</style>