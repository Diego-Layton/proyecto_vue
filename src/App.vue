<!-- SCRIPT____________________________________________________________________________________________________ -->

<script setup>
import { ref } from "vue";


let actividades=ref([]);
let actividad= ref("")
let alertax = ref("")
let fecha= ref("")
let prioridad= false
let showalerta=ref(false)
let showtable=ref(false)


function eliminar(i){
  actividades.value.splice(i,1)

  if(actividades.value.length <= 0){
    showtable.value=false
  }

}


function agregar() {
  let control = actividades.value.length;
  let fechaActual = new Date();
  let fechaSeleccionada = new Date(fecha.value + "T23:59:59");


  if (actividad.value == "") {
    showalerta.value = true;
    alertax.value = "Debe ingresar una actividad";
  } else if (fecha.value == "") {
    showalerta.value = true;
    alertax.value = "Debe ingresar una fecha";
  } else if (fechaSeleccionada < fechaActual) {
    showalerta.value = true;
    alertax.value = "La fecha no puede ser anterior a hoy";
  } else {
    let prioridadx = prioridad ? "alta" : "baja";

    actividades.value.push({
      actividad: actividad.value,
      fecha: fecha.value,
      prioridad: prioridadx
    });
    console.log(actividades);
    showtable.value = true;

    if (actividades.value.length > control) {
      actividad.value = "";
      fecha.value = "";
    }
  }
}




function ordenar() {
  return actividades.value.sort((a, b) => {
    if (a.prioridad === "alta") return -1;
    if (b.prioridad === "alta") return 1;
    return 0;
  })}
 

function closeAlert() {
showalerta.value=false
}



</script>




<!-- MAQUETADO_________________________________________________________________________________________________ -->

<template>
  <div class="contenedor">

    <div class="encabezado">
    <input type="text" placeholder="Ingrese actividad" v-model="actividad" style="width:40vmax;" />
    <input type="date" placeholder="Facha de nacimiento" v-model.trim="fecha" style="width:20vmax;"/>

  </div>

    <div class="botones">
      <button @click="agregar()" style="background-color: green; font-size: large;">+</button>

<div class="check">
      <input type="checkbox" v-model="prioridad" id="check"/>
      <label for="check"> Alta Prioridad</label>
    </div>
      <button @click="ordenar()">ORDENAR</button>
    </div>

    <table style="margin-top:1vmax;" v-if="showtable">
      <thead>
        <tr>
          <th>Actividad</th>
          <th>Prioridad</th>
          <th>Fecha</th>
          <th>Eliminar</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="(item, i) in actividades"
          :key="i"
          :style="item.prioridad=='alta'? {backgroundColor:'red' , color:'white'}:{backgroundColor:'rgb(212, 84, 84)' , color:'white'}">
          <td>{{ item.actividad }}</td>
          <td>{{ item.prioridad }}</td>
          <td>{{ item.fecha}}</td>
          <td>
          <button @click="eliminar(i)">❌</button>
        </td>
        </tr>
      </tbody>
    </table>

<div class="alerta"  style="background-color: rgb(216, 216, 216);" v-if="showalerta">
     <h3 style="font-size: x-large; color:black"> {{alertax}} 🙄</h3>
<button @click="closeAlert()" v-show="alertax"  style="background-color: darkred; width:8vmax"> Aceptar</button>
</div>
  </div>
</template>



<!-- ESTILOS____________________________________________________________________________________________________ -->

<style scoped>
.contenedor{
display: flex;
flex-direction:column;
width: 70vmax;
gap: 4vmax;

}

.botones{
  display:flex ;
  justify-content:end;
  gap:1vmax;
  align-items: center;
}

.check{
display: flex;
}
#check{
  width: 3vmin;
}
button{
  background-color: rgb(154, 154, 154);

}

.encabezado{
display: flex;
flex-direction: column;
gap: 3vmax;
margin-bottom: 3vmax;
box-shadow: 10px 10px 5px rgba(77, 76, 76, 0.76);
padding: 1vmax;
width:fit-content;
}
.alerta {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 30vmax;
  height: 15vmax;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-shadow: 1px 1px 15px 5px rgba(77, 76, 76, 0.76);
  padding: 1vmax;
  border-radius: 3vmax;
  justify-content: center;
  gap: 3vmax;
}


</style>
