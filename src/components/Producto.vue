<template>
  <tr>
    <td class="no">{{infoProducto.id}}</td>
    <td class="text-left">
      <h3>
        {{infoProducto.nombre}}
      </h3>
    </td>
    <td class="unit">${{infoProducto.precio}}</td>
    <td class="qty">{{infoProducto.cantidad}}</td>
    <td class="total">${{infoProducto.total}}</td>
    <td class="centrar" v-if="estadosApp.agregarFactura"><button type="button" @click="eliminar(infoProducto)" class="btn btn-danger centrar" >
      x
    </button></td>
  </tr>
</template>

<script>
  const encontrarIdentificacion = ({ identificacion }, identificacionBusqueda) => identificacion === identificacionBusqueda;
  export default {
    name: "Producto",
    props: {
      infoProducto:Object,
      estadosApp:Object
    },
    data: function () {
      return {
      }
    },
    methods: {
      eliminar: function(producto){
        this.$emit('eliminarProducto',producto);
      }
    }
  }
</script>

<style>

</style>
