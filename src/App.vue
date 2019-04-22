<template>
  <div id="app">
    <menuFactura :estadosApp="estadosApp"></menuFactura>
    <factura v-if="estadosApp.agregarFactura || estadosApp.buscarFactura" :listaProductos="listaProductos" :estadosApp="estadosApp" :listaClientes="listaClientes" :listaFacturas="listaFacturas"></factura>
    <agregarCliente v-if="estadosApp.agregarCliente" :listaClientes="listaClientes"></agregarCliente>
    <agregarProducto v-if="estadosApp.agregarProducto" :listaProductos="listaProductos"></agregarProducto>
  </div>
</template>

<script>
  import Vue from 'vue';
  import vSelect from 'vue-select';
  import 'vue-select/dist/vue-select.css';
  import AgregarCliente from './components/AgregarCliente.vue';
  import MenuFactura from './components/MenuFactura.vue';
  import Factura from './components/Factura.vue';
  import AgregarProducto from './components/AgregarProducto.vue';
  Vue.component('v-select', vSelect);
  import BootstrapVue from 'bootstrap-vue';
  Vue.use(BootstrapVue);
  import 'bootstrap/dist/css/bootstrap.css';
  import 'bootstrap-vue/dist/bootstrap-vue.css';
  export default {
    name: 'app',
    data() {
      return {
        titulo: "Reto factura Vue",
        listaProductos: [
          {
            nombre:'Seleccione',
            stock:0,
            precio: 0,
            id:0
          },
          {
            nombre: 'Tornillos',
            stock: 20,
            precio: 300,
            id: 1
          },
          {
            nombre: 'Clavos',
            stock: 30,
            precio: 200,
            id: 2
          },
          {
            nombre: 'Tubo',
            stock: 40,
            precio: 500,
            id: 3
          }
        ],
        estadosApp: {
          agregarCliente: false,
          agregarProducto: false,
          agregarFactura: false,
          buscarFactura: false
        },
        listaClientes: [
          {
            nombre: 'Pacho',
            apellido: 'Cifuentes',
            correo: 'pacho@gmail.com',
            identificacion: '11',
            celular: '320111'
          },
          {
            nombre: 'Andres',
            apellido: 'Pelaez',
            correo: 'andre@gmail.com',
            identificacion: '22',
            celular: '3222'
          }
        ],
        listaFacturas: []
      }
    },
    components: {
      AgregarCliente,
      MenuFactura,
      Factura,
      AgregarProducto
    },
    methods: {

    }, beforeMount() {
      this.estadosApp.agregarFactura = true;
    },
    filters: {
      fechaFormato: function (date) {
        return date.getDate() + "-" + (date.getMonth() + 1) + "-" + date.getFullYear() + ", " + date.getHours() + ":" + date.getMinutes();
      }
    }
  }
</script>

<style>
  .centrar{
    text-align: center;
  }
</style>
