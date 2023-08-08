<template>
  <div class="body">
    <div class="container">
      <button style="margin: 20px 0px;" data-bs-toggle="modal" data-bs-target="#exampleModal" id="boton">Agregar
        Producto</button>
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
              <div class="codigodiv" id="divs" ref="codigodivRef">
                <label>{{ Error_c }}</label>
                <input type="text" v-model="Codigo" id="Codigo" class="cajas">
              </div>
              <div class="nombrediv" id="divs" ref="nombredivRef">
                <label>{{ Error_n }}</label>
                <input type="text" v-model="Nombre" id="Nombre" class="cajas">
              </div>

              <div class="cantidaddiv" id="divs" ref="cantidaddivRef">
                <label for="Cantidad">{{ Error_ca }}</label>

                <input type="number" v-model="Cantidad" id="Cantidad" class="cajas">
              </div>

              <div class="preciodiv" id="divs" ref="preciodivRef">
                <label>{{ Error_p }}</label>
                <input type="number" v-model="Precio" id="Precio" class="cajas">
              </div>

              <div class="costodiv" id="divs" ref="costodivRef">

                <label>{{ Error_co }}</label>

                <input type="number" v-model="Costo" id="Costo" class="cajas">
              </div>



            </div>

            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" id="botonm">Cerrar</button>
              <button type="button" class="btn btn-primary" @click="agregarProducto()" id="botonm">{{ Boton }}</button>
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
        <tbody class="tbody">
          <tr v-for="(producto, i) in productos" :key="i">
            <td>{{ producto.Codigo }}</td>
            <td>{{ producto.Nombre }}</td>
            <td
              :style="producto.Cantidad < 10 ? 'border: 4px solid red; background-color: rgb(255, 121, 121);' : producto.Cantidad > 50 ? 'border: 4px solid blue ;background-color: rgb(124, 177, 255);' : ''">
              {{ producto.Cantidad }}</td>
            <td>{{ producto.Precio }}</td>
            <td>{{ producto.Costo }}</td>
            <td>{{ producto.Precio * producto.Cantidad }}</td>
            <td>{{ producto.Costo * producto.Cantidad }}</td>
            <td>{{ producto.Precio * producto.Cantidad - producto.Costo * producto.Cantidad }}</td>
            <td>
              <button @click="Eliminar(i)" id="Elibtn">❌</button>
              <button @click="Editar(producto, i)" id="Edibtn" data-bs-toggle="modal"
                data-bs-target="#exampleModal">✍️</button>
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
const Cantidad = ref("")
const Precio = ref("")
const Costo = ref("")



let Boton = ref("Agregar")

const productos = ref([
  { Codigo: "001", Nombre: "Producto 1", Cantidad: 5, Precio: 10, Costo: 10 },
  { Codigo: "002", Nombre: "Producto 2", Cantidad: 10, Precio: 15, Costo: 20 },
  { Codigo: "003", Nombre: "Producto 3", Cantidad: 15, Precio: 122, Costo: 30 }
]);



/* const modalAbierto = ref(false);  */
let indiceEdicion = ref(-1);

function agregarProducto() {
  if (validar()) {
    if (Boton.value === "Editar") {
      if (indiceEdicion.value >= 0) {
        const productoEditado = {
          Codigo: Codigo.value,
          Nombre: Nombre.value,
          Cantidad: Cantidad.value,
          Precio: Precio.value,
          Costo: Costo.value,

        };


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

      productos.value.push(nuevoProducto);
      Limpiar();
      cerrarModal();
    }
  }
}


function Eliminar(eli) {
  productos.value.splice(eli, 1);
}

function Limpiar() {
  Codigo.value = "";
  Nombre.value = "";
  Cantidad.value = "";
  Precio.value = "";
  Costo.value = "";
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


let codigodivRef = ref();
let nombredivRef = ref();
let cantidaddivRef = ref();
let preciodivRef = ref();
let costodivRef = ref();
let Error_c = ref("Codigo")
let Error_n = ref("Nombre")
let Error_ca = ref("Cantidad")
let Error_p = ref("Precio")
let Error_co = ref("Codigo")




function validar() {
  let validation = true;
  if (Codigo.value.trim() == "") {
    Error_c.value = "Por favor digite el codigo del producto";
    codigodivRef.value.style.border = "5px solid red";
    codigodivRef.value.style.backgroundColor = "rgb(255, 179, 179)";

    setTimeout(function () {
      Error_c.value = "Codigo";
      codigodivRef.value.style = "";
    }, 2500)
    validation = false;
  }

  else if (Nombre.value.trim() == "") {
    Error_n.value = "Por favor digite el nombre del producto";
    nombredivRef.value.style.border = "5px solid red";
    nombredivRef.value.style.backgroundColor = "rgb(255, 179, 179)";

    setTimeout(function () {
      Error_n.value = "Nombre";
      nombredivRef.value.style = "";
    }, 2500)
    validation = false;
  }



  else if ((isNaN(parseFloat(Cantidad.value)) || parseFloat(Cantidad.value) <= 0)) {
    Error_ca.value = "Por favor digite la cantidad del producto";
    cantidaddivRef.value.style.border = "5px solid red";
    cantidaddivRef.value.style.backgroundColor = "rgb(255, 179, 179)";
    
    setTimeout(function () {
      Error_ca.value = "Cantidad";
      cantidaddivRef.value.style = "";
    }, 2500)
    validation = false;
  }

  else if ((isNaN(parseFloat(Precio.value)) || parseFloat(Precio.value) <= 0)) {
    Error_p.value = "Por favor digite el precio del producto";
    preciodivRef.value.style.border = "5px solid red";
    preciodivRef.value.style.backgroundColor = "rgb(255, 179, 179)";
    
    setTimeout(function () {
      Error_p.value = "Precio";
      preciodivRef.value.style = "";
    }, 2500)
    validation = false;
  }

  else if ((isNaN(parseFloat(Costo.value)) || parseFloat(Costo.value) <= 0)) {
    Error_co.value = "Por favor digite el costo del producto";
    costodivRef.value.style.border = "5px solid red";
    costodivRef.value.style.backgroundColor = "rgb(255, 179, 179)";
    
    setTimeout(function () {
      Error_co.value = "Costo";
      costodivRef.value.style = "";
    }, 2500)
    validation = false;
  }
  return validation;
}

/* function validar() {
  let validation = true
  if (Codigo.value.trim() == "") {
    setTimeout(function () {
      Error_c.value = "Por favor digite el codigo del producto";
      codigodivRef.value.style.border = "5px solid red";
      codigodivRef.value.style.backgroundColor = "rgb(255, 179, 179)";
      setTimeout(function () {
        Error_c.value = "Codigo"
        codigodivRef.value.style = ""
      }, 3000)
    })
    validation = false
  }
  else if (Nombre.value.trim() == "") {
    setTimeout(function () {
      Error_n.value = "Por favor digite el nombre del producto";
      nombredivRef.value.style.border = "5px solid red";
      nombredivRef.value.style.backgroundColor = "rgb(255, 179, 179)";
      setTimeout(function () {
        Error_n.value = "Nombre"
        nombredivRef.value.style = ""
      }, 3000)
    })
    validation = false
  }
  else if (Cantidad.value.trim() == "") {
    setTimeout(function () {
      Error_ca.value = "Por favor digite la cantidad del producto";
      cantidaddivRef.value.style.border = "5px solid red";
      cantidaddivRef.value.style.backgroundColor = "rgb(255, 179, 179)";
      setTimeout(function () {
        Error_ca.value = "Cantidad"
        cantidaddivRef.value.style = ""
      }, 3000)
    })
    validation = false
  }
  else if (Precio.value.trim() == "") {
    setTimeout(function () {
      Error_p.value = "Por favor digite el precio del producto";
      preciodivRef.value.style.border = "5px solid red";
      preciodivRef.value.style.backgroundColor = "rgb(255, 179, 179)";
      setTimeout(function () {
        Error_p.value = "Precio"
        preciodivRef.value.style = ""
      }, 3000)
    })
    validation = false
  }
  else if (Costo.value.trim() == "") {
    setTimeout(function () {
      Error_co.value = "Por favor digite el costo del producto";
      costodivRef.value.style.border = "5px solid red";
      costodivRef.value.style.backgroundColor = "rgb(255, 179, 179)";
      setTimeout(function () {
        Error_co.value = "Costo"
        costodivRef.value.style = ""
      }, 3000)
    })
    validation = false
  }
  return validation
}
 */
</script>

<style scoped>
.body {
  padding: 30px;
  border-radius: 20px;

}

.tbody {
  border-bottom-left-radius: 100px;
}

.modal-body {
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

#Edibtn:hover {
  background-color: rgb(90, 255, 178);
  transform: scale(1.1);
  border: 3px solid green;
}

#Elibtn:hover {
  background-color: rgb(255, 179, 179);
  transform: scale(1.1);
  border: 3px solid red;
  
}

#Elibtn {
  border: none;
  background-color: rgb(255, 215, 215);
  border-radius: 10px;
}

#Edibtn {
  border: none;
  background-color: rgb(133, 255, 200);
  border-radius: 10px;
}

#th1 {
  border-top-left-radius: 25px;
}

#th9 {
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
  font-size: 25px;
  background-color: rgb(184, 228, 255);
  cursor: pointer;
  color: black;
}
#botonm{
  font-weight: bold;
  padding: 7px;
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  border-radius: 10px;
  border: none;
  font-size: 15px;
  background-color: rgb(184, 228, 255);
  cursor: pointer;
  color: black;
}

tr:nth-child(even) {
  background-color: rgb(184, 228, 255);
  /* Color para las filas impares */
}
#botonm:hover{
  background-color: rgb(135, 211, 255);
  transform: scale(1.1);
  border: 3px rgb(0, 136, 255) solid;
}
#boton:hover {
  background-color: rgb(135, 211, 255);
  transform: scale(1.1);
  border: 3px rgb(0, 136, 255) solid;

}

th {
  background-color: rgb(113, 189, 255);
  padding: 20px;
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
}
</style>