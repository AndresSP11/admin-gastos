<script setup>
defineProps(['filtro'])
defineEmits(['update:filtro'])
</script>

<template>
    <div class="filtros sombre contenedor">
        <form>
            <div class="campo">
                    <label for="nombre">Filtrar Gastos:</label>
                    <select
                    :value="filtro"
                    @input="$emit('update:filtro',$event.target.value)">
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
        </form>
    </div>


</template>


<style scoped>
.filtros{
    margin-top: 10rem;
}
.filtros .campo{
    display: flex;
    align-items: center;
    padding: 4rem;
    display: flex;
    justify-content: space-between;
}
.filtros label{
    font-size: 3rem;
    font-weight: 700;
    color: var(--gris-oscuro);
}
.filtros select{
    border-radius: 2rem;
    cursor: pointer;
    text-align: center;
    border: solid 2px var(--azul);
    width: 65%;
    padding: 1rem;
}
</style>
