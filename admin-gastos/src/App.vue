<script setup>
import ControlPresupuesto from './components/ControlPresupuesto.vue';
import Presupuesto from './components/Presupuesto.vue'
import { ref,reactive, watch,computed} from 'vue'
import iconoNuevoGasto from './assets/img/nuevo-gasto.svg'
import Modal from './components/Modal.vue'
/*Este generr Id s un generado de Id de forma unica*/
import { generarId } from './assets/helpers'
import Gasto from './components/Gasto.vue'
import Filtro from './components/Filtro.vue'
/*Presupuesto y disponible van a tener que ir de la mano*/
const presupuesto=ref(0);
const disponible=ref(0);
const gastado=ref(0);
const filtro=ref('');
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
watch(gastos,()=>{
  const totalGastado= gastos.value.reduce((total,gasto)=>gasto.cantidad + total, 0)
  console.log(totalGastado)
  gastado.value=totalGastado
  disponible.value=presupuesto.value-gastado.value;
},{
  deep:true
})
const reiniciarStateGasto=()=>{
  Object.assign(gasto,{
    nombre:'',
    cantidad:'',
    categoria:'',
    id:null,
    fecha:Date.now()
  })
}
/* Sirve para reiniciar el state de todo los valores del objeto */
watch(modal,()=>{
    if(!modal.mostrar){
      reiniciarStateGasto()
    }
},{
  deep:true
})
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
const guardarGasto=(id)=>{
  if(gasto.id){
    /* Extraemos el id, del elemento reactivo que se encuentra ahì */
    const { id } = gasto
    const i= gastos.value.findIndex((gasto=>gasto.id==id))
    gastos.value[i]={...gasto}
  }else{
    gastos.value.push({
      ...gasto,
        id:generarId()
      }
    )
  }

  cerrarModal();
  reiniciarStateGasto();
}
/* Esta funcion me permite ver el estado en el que se encuentra la parte seleccionada, el objeto seleccionado
Osea de aqui se obtiene el id, nombre etc... es por ellos que se esta utilizando la parte de filter para obtner el valor */
const seleccionarGasto=(id)=>{
  /* Aqui el filter te va botar un arreglo de demasiado elementos ,e pero en e este caso como un es un arreglo de un solo elemento, solo te devolverá uno... */
  /* Lo que esta haciedn oeta linea de codigo, e que cuando presionemos el nombre, automaticamente se haga el autollenado del formulario, de la palabra gasto, de 
  esta forma se conocerá mejor ello... */
  /* Recordando tambien que en cada input esta el valor dinamico, de esta forma e valor que es agregado en cada atributo del objeto, este se asignará automaticamente.   */
  const gastoEditar=gastos.value.filter(gasto=>gasto.id===id)[0]
  
  
  Object.assign(gasto,gastoEditar);
  mostrarmodal()
}

const eliminarGasto=()=>{
  gastos.value=gastos.value.filter(gastoState=>gastoState.id!==gasto.id)
  cerrarModal();
}

const gastosFiltrados = computed(()=>{
  if(filtro.value){
    return gastos.value.filter(gasto=>gasto.categoria===filtro.value)
  }
  return gastos.value
})


</script>

<template>
  <div
  :class="{ fijar: modal.mostrar }"
  >
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
      :disponible="disponible"
      :gastado="gastado"></ControlPresupuesto>
      </div>
      
      
    </header>
    <main v-if="presupuesto>0">

      <Filtro
      v-model:filtro="filtro"
      >
          </Filtro>  
      <div class="listado-gastos contenedor">
          
          
          <h2>{{ gastos.length > 0 ? 'Gastos' : 'No hay gastos' }}</h2>

          <Gasto
          v-for="gasto in gastosFiltrados"
          :key="gasto.id"
          :gasto="gasto"
          
          @seleccionar-gasto="seleccionarGasto"
          ></Gasto>
        </div>

        <div class="crear-gasto">
          <!--  -->
          <img :src="iconoNuevoGasto" alt="icono nuevo gasto"
          
          @click="mostrarmodal">
        </div>
        <!-- AQUI SE ESTA MANDANDO LA FUNCION PERO AQUI SE ESTA MODIFICANDO LOS VALORES DEL MISMO MODAL -->
        <Modal v-if="modal.mostrar==true"
       
        @cerrar-modal="cerrarModal"
        @guardar-gasto="guardarGasto"
        @eliminar-gasto="eliminarGasto"
        :modal="modal"
        :disponible="disponible"
        :id="gasto.id"
        v-model:nombre="gasto.nombre"
        v-model:cantidad="gasto.cantidad"
        v-model:categoria="gasto.categoria"></Modal>
      </main>
  </div>
</template>

<style>
  .fijar{
    overflow: hidden;
    height: 100vh;
  }
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
  .listado-gastos{
    margin-top: 10rem;
  }
  .listado-gastos h2{
    font-weight: 900;
    color: var(--gris-oscuro);
  }
</style>
