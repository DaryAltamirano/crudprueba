<template>
  <div class="modal " v-bind:id=id tabindex="-1">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">{{operacion}} Producto</h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
        <form>
          <div class="mb-3">
            <!-- <label for="recipient-name" class="col-form-label">Nombre :</label> -->
            <input type="text" class="form-control" id="" v-model="valorNombre" placeholder="Nombre">
          </div>
          <div class="mb-3">
            <!-- <label for="message-text" class="col-form-label">Descripcion :</label> -->
            <textarea class="form-control" id="" v-model="valorDescripcion" placeholder="Descripcion" >  </textarea>
          </div>
          <div class="col">
          <!-- <label for="message-text" class="col-form-label">precio</label> -->
          <input type="decimal" class="form-control" v-model="precio" placeholder="Precio">
        </div>
        </form>
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            Salir
          </button>
          <button v-on:click=ElegirMethod(idp) type="button" class="btn btn-primary" data-bs-dismiss="modal">{{save}}</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios'
export default {
  name: "Modal",
  data() {
    return {
      valorNombre:this.nombre,
      valorDescripcion:this.descripcion,
      precio:this.precios,
    };
  },
  props: ["operacion","nombre", "descripcion","precios","id","save","idp"],
  methods:{
      toggleModal () {
        this.modal = !this.modal
      },
      createProducto(){
        var data = {
          nombre: this.valorNombre,
          descripcion:this.valorDescripcion,
          precio:this.precio,
          };
        axios.post('http://127.0.0.1:8000/productos/',data)
        .then(res => res.json())
          .then(res=> console.log(res))
        .catch(error => console.error('Error:', error))
      },
      editProducto(id){
          var data = {
          nombre: this.valorNombre,
          descripcion:this.valorDescripcion,
          precio:this.precio,
          }
        fetch('http://127.0.0.1:8000/productos/' + id, {
         method: 'PUT', 
          body: JSON.stringify(data), 
           headers:{
           'Content-Type': 'application/json'
          }})
          .then(res => res.json())
          .then(res=> console.log(res))
        .catch(error => console.error('Error:', error))
      },
      ElegirMethod(id){
        if(!this.idp){
          this.createProducto();
        }else{
           this.editProducto(id);
        }
        this.$emit('escucharhijo');
      }
  },
};
</script>

<style>
</style>