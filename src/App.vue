<template>
  <div class="body">
    <div class="container">
      <button style="margin: 20px 0px;" data-bs-toggle="modal" data-bs-target="#exampleModal" id="boton">Agregar Producto</button>
      <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h1 class="modal-title fs-5" id="exampleModalLabel">Nuevo registro</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              <h4 id="Alerta"></h4>
            </div>
            <div class="modal-body">

              <h1 class="title">Datos de la tabla</h1>
              <div class="codigodiv" id="divs">
                <label>Código</label>
                <h1 v-text="Error_codigo"></h1>
                <input type="text" v-model="Codigo" id="Codigo" class="cajas">
              </div>
              <div class="nombrediv" id="divs">
                <label for="Nombre">Nombre</label>
                <h1 v-text="Error_nombre"></h1>
                <input type="text" v-model="Nombre" id="Nombre" class="cajas">
              </div>

              <div class="cantidaddiv" id="divs">
                <label for="Cantidad">Cantidad</label>
                <h1 v-text="Error_cantidad"></h1>
                <input type="number" v-model="Cantidad" id="Cantidad" class="cajas">
              </div>

              <div class="preciodiv" id="divs">
                <label for="Precio">Precio</label>
                <h1 v-text="Error_precio"></h1>
                <input type="number" v-model="Precio" id="Precio" class="cajas">
              </div>

              <div class="costodiv" id="divs">

                <label for="Costo">Costo</label>
                <h1 v-text="Error_costo"></h1>
                <input type="number" v-model="Costo" id="Costo" class="cajas">
              </div>

             

            </div>

            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" id="boton" >Cerrar</button>
              <button  type="button" class="btn btn-primary" @click="agregarProducto()"
                id="boton">{{Boton}}</button>
            </div>
          </div>
        </div>
      </div>




    </div>

    <div class="tabladiv">
      <table class="tabla">
        <thead class="tablahead">
          <tr>
            <th id="th1">Código</th>
            <th>Nombre</th>
            <th>Cantidad</th>
            <th>Precio</th>
            <th>Costo</th>
            <th>Precio Total</th>
              <th>Costo Total</th>
              <th>Ganancias</th>
            <th id="th9">Opciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(producto, i) in productos" :key="i">
            <td>{{ producto.Codigo }}</td>
            <td>{{ producto.Nombre }}</td>
            <td
              :style="producto.Cantidad < 10 ? 'border: 4px solid red; background-color: rgb(255, 121, 121);' : producto.Cantidad > 50 ? 'border: 4px solid blue ;background-color: rgb(124, 177, 255);' : ''">
              {{ producto.Cantidad }}</td>
            <td>{{ producto.Precio }}</td>
            <td>{{ producto.Costo }}</td>
            <td>{{ producto.Precio*producto.Cantidad }}</td>
            <td>{{ producto.Costo * producto.Cantidad }}</td>
            <td>{{ producto.Precio * producto.Cantidad-producto.Costo*producto.Cantidad }}</td>
            <td>
              <button @click="Eliminar(i)"   id="Elibtn">❌</button>
              <button  @click="Editar(producto, i)"  id="Edibtn" data-bs-toggle="modal" data-bs-target="#exampleModal">✍️</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue';
const Codigo = ref("")
const Nombre = ref("")
const Cantidad = ref(0)
const Precio = ref(0)
const Costo = ref(0)
let Error_codigo = ref("")
let Error_nombre = ref("")
let Error_cantidad = ref("")
let Error_precio = ref("")
let Error_costo = ref("")
let codigodiv = ref("")


let Boton = ref("Agregar")

const productos = ref([
  { Codigo: "001", Nombre: "Producto 1", Cantidad: 5, Precio: 10, Costo: 10 },
  { Codigo: "002", Nombre: "Producto 2", Cantidad: 10, Precio: 15, Costo: 20 },
  { Codigo: "003", Nombre: "Producto 3", Cantidad: 15, Precio: 122, Costo: 30 }
]);



/* const modalAbierto = ref(false);  */
let indiceEdicion = ref(-1); 

function agregarProducto() {
  if (Boton.value === "Editar") {
    if (indiceEdicion.value >= 0) {
      const productoEditado = {
        Codigo: Codigo.value,
        Nombre: Nombre.value,
        Cantidad: Cantidad.value,
        Precio: Precio.value,
        Costo: Costo.value,
        
      };
      validar();

      productos.value.splice(indiceEdicion.value, 1, productoEditado);
      Boton.value = "Agregar";
      Limpiar();
      cerrarModal();
    }
  } else {
    const nuevoProducto = {
      Codigo: Codigo.value,
      Nombre: Nombre.value,
      Cantidad: Cantidad.value,
      Precio: Precio.value,
      Costo: Costo.value,
      
    };
    validar();
    productos.value.push(nuevoProducto);
    Limpiar();
    cerrarModal();
  }
}

function Eliminar(eli) {
  productos.value.splice(eli, 1);
}

function Limpiar() {
  Codigo.value = "";
  Nombre.value = "";
  Cantidad.value = 0;
  Precio.value = 0;
  Costo.value = 0;
}

function Editar(Edi, index) {
  Codigo.value = Edi.Codigo;
  Nombre.value = Edi.Nombre;
  Cantidad.value = Edi.Cantidad;
  Precio.value = Edi.Precio;
  Costo.value = Edi.Costo;
  Boton.value = "Editar";   
  indiceEdicion.value = index; 

}


let validation = false
function validar(){
  if(Codigo.value.trim()==""){
    Error_codigo.value ="Error";
    codigodiv.value.style = "border:1px solid red"


  }
}

</script>

<style scoped>
.body {
  padding: 30px;
  border-radius: 20px;

}
.modal-body{
  display: flex;
  flex-direction: column;
  gap: 15px;
  text-align: center;
  align-items: center;
  border-top: 3px solid black;
  border-bottom: 3px solid black;
  background-color: rgb(191, 255, 255);
}

.container {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 25px;
  border: 3px solid black;
  background-color: white;
  padding: 30px;
  border-radius: 30px;
  display: grid;
  gap: 15px;
  justify-items: center;
  width: 1400px;
}

#Edibtn:hover{
  background-color: rgb(90, 255, 178);
  transform:   scale(1.1);
  border: 3px solid green; 
}

#Elibtn:hover{
  background-color: rgb(255, 179, 179);
  transform:   scale(1.1);
  border: 3px solid red; 
}
#Elibtn{
 border: none;
  background-color: rgb(255, 215, 215);
}
#Edibtn{
  border: none;
  background-color: rgb(133, 255, 200);
}

#th1{
  border-top-left-radius: 25px;
}
#th9{
  border-top-right-radius: 25px;

}

.tabladiv {
  font-size: 25px;
  border: 3px solid black;
  display: flex;
  justify-content: center;
  background-color: white;
  padding: 30px;
  border-radius: 30px;
  margin-top: 30px;
}


td {
  padding: 10px;
  text-align: center;

}

.tabla {
  box-shadow: 5px 5px 114px rgb(162, 247, 255);

  border-collapse: collapse;

}

#boton {
  font-weight: bold;
  padding: 15px;
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  border-radius: 10px;
 border: none;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 25px;
  background-color: rgb(184, 228, 255);
  cursor: pointer;
  color: black;
}
tr:nth-child(even) {
  background-color: rgb(184, 228, 255); /* Color para las filas impares */
}
#boton:hover {
  background-color: rgb(135, 211, 255);
  transform:   scale(1.1);
  border: 3px rgb(0, 136, 255) solid;

}

th {
  background-color: rgb(113, 189, 255);
  padding: 10px;
  text-align: center;

}

#divs {

  display: flex;
  flex-direction: column;
  width: 200px;

  background-color: aliceblue;
  padding: 15px;
  border-radius: 20px;
  border: 3px rgb(0, 136, 255) solid;
}

button {
  margin: 5px;
  padding: 7px;
}</style>