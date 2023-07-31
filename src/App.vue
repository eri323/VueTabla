<template>
  <div class="body">
    <div class="container">
      <label for="Codigo">Código:</label>
      <input type="text" v-model="nuevoProducto.Codigo" id="Codigo" class="cajas">

      <label for="Nombre">Nombre:</label>
      <input type="text" v-model="nuevoProducto.Nombre" id="Nombre" class="cajas">

      <label for="Cantidad">Cantidad:</label>
      <input type="number" v-model="nuevoProducto.Cantidad" id="Cantidad" class="cajas">

      <label for="Precio">Precio:</label>
      <input type="number" v-model="nuevoProducto.Precio" id="Precio" class="cajas">

      <label for="Costo">Costo:</label>
      <input type="number" v-model="nuevoProducto.Costo" id="Costo" class="cajas">

      <button @click="agregarProducto" class="boton">Agregar <br> Producto</button>
    </div>

    <div class="tabladiv">
      <table class="tabla">
        <thead>
          <tr>
            <th>Código</th>
            <th>Nombre</th>
            <th>Cantidad</th>
            <th>Precio</th>
            <th>Costo</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(producto, i) in productos" :key="i">
            <td>{{ producto.Codigo }}</td>
            <td>{{ producto.Nombre }}</td>
            <td :style="producto.Cantidad<10? 'color:red':'color:blue'">{{ producto.Cantidad }}</td>
            <td>{{ producto.Precio }}</td>
            <td>{{ producto.Costo }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    
  </div>
</template>

<script setup>
import { ref } from 'vue';

const productos = ref([
  { Codigo: "001", Nombre: "Producto 1", Cantidad: 5, Precio: 10, Costo: 0},
]);

const nuevoProducto = ref({
  Codigo: "",
  Nombre: "",
  Cantidad: "",
  Precio: "",
  Costo: ""
});

function agregarProducto() {
  productos.value.push({
    Codigo: nuevoProducto.value.Codigo,
    Nombre: nuevoProducto.value.Nombre,
    Cantidad: nuevoProducto.value.Cantidad,
    Precio: nuevoProducto.value.Precio,
    Costo: nuevoProducto.value.Costo
  });
};
</script>

<style scoped>
.body{
  background-color: rgb(101, 250, 255);
  padding: 30px;
  border-radius: 20px;
}
.container{
  background-color: white;
  padding: 30px;
  border-radius: 30px;
  display: flex;
  flex-direction: column;
  gap: 5px;
  align-items: center;
}
.cajas{
  width: 100px;
}
.tabladiv{
  display: flex;
  justify-content: center;
  background-color: white;
  padding: 30px;
  border-radius: 30px;
  margin-top: 30px;
}
th, td{
  border: 3px solid black;
  padding: 10px;
  text-align: center;
  border-radius: 25px;
}
.tabla{
  border-collapse: collapse;

}
.boton{
  padding: 5px;
  margin-top: 10px;
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  border-radius: 10px;
  border: 3px solid rgb(0, 0, 0);
}
.boton :hover{
  background-color: greenyellow;
}
</style>