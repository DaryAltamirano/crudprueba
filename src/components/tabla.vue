<template>
  <div class="pt-5 mt-5 container">
    <h1 class="text-center pb-5">Crud Basico con php y Vue</h1>
    <div class="d-flex flex-row-reverse">
      <button type="button" class="btn btn-success" data-bs-toggle="modal" v-bind:data-bs-target="'#AgregarProducto'">Agregar Producto</button>
        <Modal v-bind:operacion="'Agregar'" v-bind:nombre="''" v-bind:descripcion="''" v-bind:id="'AgregarProducto'" v-bind:save="'Agregar Producto'"></Modal>
    </div>
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col" class="text-center">id</th>
          <th scope="col" class="text-center">Producto</th>
          <th scope="col" class="text-center">Descripcion</th>
          <th scope="col" class="text-center">Opciones</th>
        </tr>
      </thead>
      <tbody>
        <tr  v-for="item in productos" :key="item.id" >
          <th scope="row">{{item.id}}</th>
          <td class="col-4 text-center">{{item.nombre}}</td>
          <td class="col-4 text-center">{{item.descripcion}}</td>
          <td class="col-4 text-center">
            <button
              type="button"
              class="btn btn-warning mx-2"
              data-bs-toggle="modal"
              v-bind:data-bs-target="'#' + item.nombre"
            >
              Editar
            </button>
            <Modal @escucharhijo="variableHijo" v-bind:operacion="'Editar'" v-bind:precios=item.precio v-bind:nombre=item.nombre v-bind:descripcion=item.descripcion v-bind:idp=item.id  v-bind:id=item.nombre v-bind:save="'Guardar Producto'" ></Modal>
            <button type="button" v-on:click=EliminarProducto(item.id) class="btn btn-danger mx-2">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>


import Modal from "./modal.vue"
export default {
  name: "Tabla",
  data() {
      return{
      valorhijo:null,
      productos:[
          ]}
  },
  components: {
    Modal,
  },
   mounted(){
     this.ObtenerProducto();
  },
  methods:{
    ObtenerProducto(){
      fetch('http://127.0.0.1:8000/productos/')
      .then(respuesta=>respuesta.json())
      .then(data =>{ this.productos=data['data']; console.log(this.productos)})
      .catch(function(error) {
      console.log('Hubo un problema con la petición Fetch:' + error.message);
      });
    },
    EliminarProducto(id){
      fetch('http://127.0.0.1:8000/productos/' + id, {
      method: 'DELETE',
      })
      .then(res => res.json()) 
      .then(res => this.productos=res['data'])
    },
    variableHijo(){
       this.ObtenerProducto();
    },
  }
};
</script>

<style>
</style>












