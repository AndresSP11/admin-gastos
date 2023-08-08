<script setup>

</script>


<template>
    <div class="alerta">
        <!-- En este slot, se genera los valores ue se desean ver... pero en este caso se esta presentando uniamente un valor , en acmbioen el otro en el mismo formularpo
        se esta definiendo un objeto para psarlo como props, para ver el tipo en el que es, si es erro imprime de oclor rojo, pero si es de otro tipo imprimer de color 
        verde asi que no hay problema... -->
        <slot></slot>

    </div>
</template>


<style scoped>
.alerta{
    padding: 1rem;
    font-weight: 900;
    text-transform: uppercase;
    font-size: 1.6rem;
    text-align: center;
    background-color: var(--blanco);
    border-left: .5rem solid #b91c1c;
    color:#b91c1c;
    margin-bottom: 20px;
}

</style>