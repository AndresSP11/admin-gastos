<script setup>
import ControlPresupuesto from './components/ControlPresupuesto.vue';
import Presupuesto from './components/Presupuesto.vue'
import { ref,reactive} from 'vue'
import iconoNuevoGasto from './assets/img/nuevo-gasto.svg'
import Modal from './components/Modal.vue'
import { generarId } from './assets/helpers'
const presupuesto=ref(0);
const disponible=ref(0);
const modal=reactive({
  animar:false,
  mostrar:false
})
/* CREAMOS UN ELEMENTO DE GASTO */
/* ESTE CASO VA UTILIZAR LA PARTE DE MODAL, PARA ASIGNARLE LOS VALORES DE MODAL */
const gasto=reactive({
  nombre:'',
  cantidad:'',
  categoria:'',
  id:null,
  fecha:Date.now()
})
const gastos=ref([]);
/*Esta funciio esta utilizando los omponentes de Presupuesto y App.vue, para asignarle valor al presupuesto*/
const definirPresupuesto=(cantidad)=>{
    presupuesto.value=cantidad
    disponible.value=cantidad
}
/* 
Estas es la función que vamos a utilizar para mostrar el modal, una vez le hemos dadod click a la parte de + para que suba el modal
*/

const mostrarmodal=()=>{
  modal.animar=true
  setTimeout(()=>{
    modal.animar=true
  },600)
  modal.mostrar=true
}
/* En este caso, se esta cerrando o utilizando esta función en el mismo Modal, para cerrar dicho valor */ 
const cerrarModal=()=>{
  modal.animar=false
  setTimeout(()=>{
    modal.mostrar=false
  },500)
  
}
const guardarGasto=()=>{
  gastos.value.push({
    ...gasto,
    id:generarId()
    
  }
  )
}
</script>

<template>
  <div>
    <header>
      <h1>Planificador de Gastos</h1>
      <div class="contenedor-header contenedor sombre">
      <!-- En esta linea de codigo, se va ejecutar la parte de presupuesto, una vez se coloque 
      el elemento reacivo valor del presupuesto, que estamos enviando mediante la parte  -->
      <Presupuesto 
      v-if="presupuesto===0"
      @definir-presupuesto="definirPresupuesto">
      </Presupuesto>
      <!-- De aqui vamos a mandar los daatos del padre hacia el hijo, que en este caso vendria a dar lectura, mediante los props -->
      <ControlPresupuesto v-else
      :presupuesto="presupuesto"
      :disponible="disponible"></ControlPresupuesto>
      </div>
      
      
    </header>
    <main v-if="presupuesto>0">
        <div class="crear-gasto">
          <!--  -->
          <img :src="iconoNuevoGasto" alt="icono nuevo gasto"
          
          @click="mostrarmodal">
        </div>
        <!-- AQUI SE ESTA MANDANDO LA FUNCION PERO AQUI SE ESTA MODIFICANDO LOS VALORES DEL MISMO MODAL -->
        <Modal v-if="modal.mostrar==true"
        @cerrar-modal="cerrarModal"
        @guardar-gasto="guardarGasto"
        :modal="modal"
        v-model:nombre="gasto.nombre"
        v-model:cantidad="gasto.cantidad"
        v-model:categoria="gasto.categoria"></Modal>
      </main>
  </div>
</template>

<style>
  :root{
    --azul: #3b82f6;
    --blanco: #fff;
    --gris-claro: #f5f5f5;
    --gris: #94a3b8;
    --gris-oscuro: #64748b;
    --negro: #000;
  }
  html{
    font-size: 62.5%;
    box-sizing: border-box;
  }
  *,
  *:before,
  *:after{
    box-sizing: inherit;
  }
  body{
    font-size: 1.6rem;
    font-family: "Lato", sans-serif;
    background-color: var(--gris-claro);
  }
  h1{
    font-size:4rem;
  }
  h2{
    font-size:3rem;
  }
  header{
    background-color: var(--azul);
  }
  header h1{
    padding: 3rem 0;
    margin: 0;
    color: var(--blanco);
    text-align: center;
  }
  .contenedor{
    width: 90%;
    max-width: 80rem;
    border-radius: 2rem;
    margin: 0 auto;
    background-color: rgb(255, 255, 255);
  }
  .contenedor-header{
    margin-top: -5rem;
    transform: translateY(5rem);
    padding: 5rem;
  }
  .sombre{
    box-shadow:0px 10px 15px -3px rgba(0, 0, 0, 0.1)
  }
  .crear-gasto{
    position: fixed;
    bottom: 5rem;
    right: 5rem;
  }
  .crear-gasto img{
    width: 5rem;
  }
  .crear-gasto img:hover{
    cursor: pointer;
  }
</style>
