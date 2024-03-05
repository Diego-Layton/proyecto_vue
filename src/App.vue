<!-- SCRIPT____________________________________________________________________________________________________ -->

<script setup>
import { ref } from "vue";


let actividades=ref([]);
let actividad= ref("")
let alertax = ref("")
let fecha= ref("")
let prioridad= ref(false)
let prioridadx= ""

function eliminar(i){
  actividades.value.splice(i,1)
}


function agregar() {



if (actividad.value=="") {
alertax.value="Debe ingresar una actividad"
}
else if (fecha.value=="") {
alertax.value="Debe ingresar una fecha"
}

else{

if (this.prioridad)
prioridadx="alta"
else
prioridadx="baja"

actividades.value.push({
actividad: actividad.value,
fecha: fecha.value,
prioridad: prioridadx
})
console.log(actividades);

}}


function ordenar() {
  return actividades.value.sort((a, b) => {
    if (a.prioridad === "alta") return -1;
    if (b.prioridad === "alta") return 1;
    return 0;
  })}
 

function closeAlert() {
alertax.value=""
}



</script>




<!-- MAQUETADO_________________________________________________________________________________________________ -->

<template>
  <div class="contenedor">
    <input type="text" placeholder="Ingrese actividad" v-model="actividad" style="width:40vmax;" />
    <input type="date" placeholder="Facha de nacimiento" v-model.trim="fecha" style="width:20vmax;"/>

    <div class="botones">
      <button @click="agregar()" style="background-color: green; font-size: large;">+</button>

<div class="check">
      <input type="checkbox" v-model="prioridad" id="check"/>
      <label for="check"> Alta Prioridad</label>
    </div>
      <button @click="ordenar()">ORDENAR</button>
    </div>

    <table style="margin-top:1vmax;">
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
          :style="item.prioridad=='alta'? {backgroundColor:'red' , color:'white'}:''">
          <td>{{ item.actividad }}</td>
          <td>{{ item.prioridad }}</td>
          <td>{{ item.fecha}}</td>
          <td>
          <button @click="eliminar(i)">❌</button>
        </td>
        </tr>
      </tbody>
    </table>


    <textarea v-if="alertax" v-model="alertax" style="background-color: salmon; font-size: x-large;"></textarea>
<button @click="closeAlert()" v-show="alertax"  style="background-color: darkred;"> Aceptar</button>
  </div>
</template>



<!-- ESTILOS____________________________________________________________________________________________________ -->

<style scoped>
.contenedor{
display: flex;
flex-direction:column;
width: 70vmax;
gap: 1vmax;
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
</style>
