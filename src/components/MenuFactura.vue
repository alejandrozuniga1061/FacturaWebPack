<template>
    <div>
        <b-nav pills>
          <b-nav-item>Inicio</b-nav-item>
          <b-nav-item-dropdown
            id="my-nav-dropdown"
            text="Factura"
            extra-toggle-classes="nav-link-custom"
            right
          >
            <b-dropdown-item @click="navegar('agregarFactura')">Nuevo</b-dropdown-item>
            <b-dropdown-item @click="navegar('buscarFactura')">Buscar</b-dropdown-item>
          </b-nav-item-dropdown>
          <b-nav-item-dropdown
            id="my-nav-dropdown"
            text="Otros"
            extra-toggle-classes="nav-link-custom"
            right
          >
            <b-dropdown-item @click="navegar('agregarCliente')"> Nuevo cliente</b-dropdown-item>
             <b-dropdown-item @click="navegar('agregarProducto')">Nuevo producto</b-dropdown-item>
            <b-dropdown-divider></b-dropdown-divider>
          </b-nav-item-dropdown>
        </b-nav>
      </div>
</template>

<script>
  export default {
    name: "MenuFactura",
    props: {estadosApp:Object},
    data: function () {
      return {

      }
    },
    methods: {
      navegar:function (item){
        this.cerrarTodo();
        let _self = this;
        setTimeout(function(){
          _self.estadosApp[item] = true;
        }, 100);
      },
      cerrarTodo: function () {
        for (var key in this.estadosApp) {
          this.estadosApp[key] = false;
        }
      }
    }
  }
</script>

<style>
</style>
