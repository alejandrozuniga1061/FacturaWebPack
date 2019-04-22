<template>
  <div class="invoice overflow-auto">
    <div style="min-width: 600px">
      <header>
        <div class="row">
          <div class="col" style="text-align: center">
            <img src="dist/ferreteria.jpg" style="width: 20%;" alt="" />
          </div>
          <div class="col company-details">
            <h2 class="name">
              Factura #{{factura.numeroFactura}}
            </h2>
            <div>Calle Falsa 123</div>
            <div>(320) 929394</div>
            <div>factura@reto.com</div>
          </div>
        </div>
      </header>
      <main>
        <!-- Buscar cliente para crear factura -->
        <div class="row" v-if="estadosApp.agregarFactura">
          <div class="col-md-4 col invoice-to">
            Seleccione un cliente para iniciar factura
          </div>
          <div class="col-md-4">
            <v-select label="identificacion" v-model="factura.cliente" :options="listaClientes"></v-select>
          </div>
        </div>
        <!-- Buscar factura -->
        <div class="row" v-if="estadosApp.buscarFactura">
            <div class="col-md-4 col invoice-to">
              Ingrese el número de la factura
            </div>
            <div class="col-md-4">
              <v-select label="numeroFactura" v-model="factura" :options="listaFacturas"></v-select>
            </div>
        </div>
        <div class="row contacts" v-if="factura.cliente.identificacion">
          <div class="col invoice-to">
            <div class="text-gray-light">A NOMBRE DE:</div>
            <h2 class="to">{{factura.cliente.nombre + ' ' + factura.cliente.apellido}}</h2>
            <div class="email"><a>{{factura.cliente.correo}}</a></div>
          </div>
          <div class="col invoice-details">
            <h1 class="invoice-id">Fecha factura</h1>
            <div class="date">{{new Date() | fechaFormato}}</div>
          </div>
        </div>
        <br>
        <table border="0" cellspacing="0" cellpadding="0" v-if="factura.cliente.identificacion">
          <thead>
            <tr>
              <th>Cod.</th>
              <th class="text-left">DESCRIPCIÓN</th>
              <th class="text-right">PRECIO</th>
              <th class="text-right">CANTIDAD</th>
              <th class="text-right">TOTAL</th>
              <th class="centrar" v-if="estadosApp.agregarFactura">OPCION</th>
            </tr>
          </thead>
          <tbody>
            <tr v-if="estadosApp.agregarFactura">
              <td class="no"></td>
              <td class="text-left">
                <v-select label="nombre" v-model="productoSeleccionado" :options="listaProductos"></v-select>
              </td>
              <td class="unit">${{productoSeleccionado.precio | numeroDefecto}}</td>
              <td class="qty"><input type="text" class="form-control" id="usr" v-model="productoSeleccionado.cantidad"
                  v-on:change="calcularPrecio()"></td>
              <td class="total">${{productoSeleccionado.total | numeroDefecto}}</td>
              <td class="centrar"><button type="button" class="btn btn-success centrar" @click="agregarProducto">
                  +
                </button></td>
            </tr>
            <producto v-for="producto in factura.listaProductos" :infoProducto="producto" :estadosApp="estadosApp" v-on:eliminarProducto="eliminarProducto"></producto>
          </tbody>
          <tfoot>
            <tr>
              <td colspan="2"></td>
              <td colspan="2">SUBTOTAL</td>
              <td>${{factura.listaProductos | calcularSubTotal}}</td>
            </tr>
            <tr>
              <td colspan="2"></td>
              <td colspan="2">TOTAL CON IVA 19%</td>
              <td>${{factura.listaProductos | calcularIVA}}</td>
            </tr>
            <tr v-if="estadosApp.agregarFactura">
              <td colspan="5"><button type="button" :disabled="factura.listaProductos.length === 0" class="btn btn-success centrar" @click="guardarFactura">Guardar factura</button></td>
            </tr>
          </tfoot>
        </table>
      </main>
    </div>
    <!--DO NOT DELETE THIS div. IT is responsible for showing footer always at the bottom-->
    <div></div>
  </div>
</template>

<script>
  import Vue from 'vue';
  import vSelect from 'vue-select';
  import 'vue-select/dist/vue-select.css';
  Vue.component('v-select', vSelect);
  import BootstrapVue from 'bootstrap-vue';
  Vue.use(BootstrapVue);
  import 'bootstrap/dist/css/bootstrap.css';
  import 'bootstrap-vue/dist/bootstrap-vue.css';
  import Producto from './Producto.vue';
  const IVA = 0.19;
  export default {
    name: 'Factura',
    props: {
      listaProductos: Array,
      listaClientes: Array,
      listaFacturas: Array,
      estadosApp: Object
    },
    data() {
      return {
        productoSeleccionado: {},
        factura: {
          numeroFactura: this.listaFacturas.length + 1,
          listaProductos: [],
          cliente: {}
        }
      }
    },
    components: {
      Producto
    },
    methods: {
      calcularPrecio: function () {
        this.productoSeleccionado.total = this.productoSeleccionado.cantidad * this.productoSeleccionado.precio;
        this.$forceUpdate()
      },
      agregarProducto: function () {
        if (this.validarProducto()) {
          this.factura.listaProductos.push({...this.productoSeleccionado});
          this.gestionarProducto();
        } else {
          alert("Verificar datos obligatorios y stock del producto");
        }
      },
      gestionarProducto: function () {
        this.productoSeleccionado.stock -= this.productoSeleccionado.cantidad;
        this.limpiarProductoSeleccionado();
      },
      validarProducto: function () {
        return this.productoSeleccionado.cantidad !== "" && this.productoSeleccionado.id !== ""
          && this.productoSeleccionado.cantidad && this.productoSeleccionado.id
          && this.productoSeleccionado.cantidad > 0
          && this.productoSeleccionado.stock >= this.productoSeleccionado.cantidad;
      },
      guardarFactura: function (){
        this.listaFacturas.push(Object.assign({}, this.factura));
        alert("Se guardó la factura con éxito");
        this.factura = {
          listaProductos: [],
          cliente: {},
          numeroFactura: this.listaFacturas.length + 1
        }
        this.limpiarProductoSeleccionado();
      },
      limpiarProductoSeleccionado: function() {
        this.productoSeleccionado.cantidad = '';
        this.productoSeleccionado.total = '';
        this.productoSeleccionado = this.listaProductos[0];
      },
      eliminarProducto:function(producto) {
        var prod = this.listaProductos.find(a=>a.id === producto.id);
        prod.stock = parseFloat(prod.stock)+ parseFloat(producto.cantidad);
        this.factura.listaProductos = this.factura.listaProductos.filter(a=>a.id !== producto.id);
      }
    },
    filters: {
      fechaFormato: function (date) {
        return date.getDate() + "-" + (date.getMonth() + 1) + "-" + date.getFullYear() + ", " + date.getHours() + ":" + date.getMinutes();
      },
      numeroDefecto: function (numero) {
        return numero || 0;
      },
      calcularSubTotal: function (productos = [0]) {
        if(productos.length > 0){
          return Object.keys(productos).reduce(function (previous, key) {
            return previous + productos[key].total;
          }, 0);
        }
      },
      calcularIVA: function (productos = [0]){
        if(productos.length > 0){
          var precio = Object.keys(productos).reduce(function (previous, key) {
            return previous + productos[key].total;
          }, 0);
          return precio + (precio * IVA);
        }
      }
    }
  }
</script>

<style>
  #invoice {
    padding: 30px;
  }

  .invoice {
    position: relative;
    background-color: #FFF;
    min-height: 680px;
    padding: 15px
  }

  .invoice header {
    padding: 10px 0;
    margin-bottom: 20px;
    border-bottom: 1px solid #3989c6
  }

  .invoice .company-details {
    text-align: right
  }

  .invoice .company-details .name {
    margin-top: 0;
    margin-bottom: 0
  }

  .invoice .contacts {
    margin-bottom: 20px
  }

  .invoice .invoice-to {
    text-align: left
  }

  .invoice .invoice-to .to {
    margin-top: 0;
    margin-bottom: 0
  }

  .invoice .invoice-details {
    text-align: right
  }

  .invoice .invoice-details .invoice-id {
    margin-top: 0;
    color: #3989c6
  }

  .invoice main {
    padding-bottom: 50px
  }

  .invoice main .thanks {
    margin-top: -100px;
    font-size: 2em;
    margin-bottom: 50px
  }

  .invoice main .notices {
    padding-left: 6px;
    border-left: 6px solid #3989c6
  }

  .invoice main .notices .notice {
    font-size: 1.2em
  }

  .invoice table {
    width: 100%;
    border-collapse: collapse;
    border-spacing: 0;
    margin-bottom: 20px
  }

  .invoice table td,
  .invoice table th {
    padding: 15px;
    background: #eee;
    border-bottom: 1px solid #fff
  }

  .invoice table th {
    white-space: nowrap;
    font-weight: 400;
    font-size: 16px
  }

  .invoice table td h3 {
    margin: 0;
    font-weight: 400;
    color: #3989c6;
    font-size: 1.2em
  }

  .invoice table .qty,
  .invoice table .total,
  .invoice table .unit {
    text-align: right;
    font-size: 1.2em
  }

  .invoice table .no {
    color: #fff;
    font-size: 1.6em;
    background: #3989c6
  }

  .invoice table .unit {
    background: #ddd
  }

  .invoice table .total {
    background: #3989c6;
    color: #fff
  }

  .invoice table tbody tr:last-child td {
    border: none
  }

  .invoice table tfoot td {
    background: 0 0;
    border-bottom: none;
    white-space: nowrap;
    text-align: right;
    padding: 10px 20px;
    font-size: 1.2em;
    border-top: 1px solid #aaa
  }

  .invoice table tfoot tr:first-child td {
    border-top: none
  }

  .invoice table tfoot tr:last-child td {
    color: #3989c6;
    font-size: 1.4em;
    border-top: 1px solid #3989c6
  }

  .invoice table tfoot tr td:first-child {
    border: none
  }

  .invoice footer {
    width: 100%;
    text-align: center;
    color: #777;
    border-top: 1px solid #aaa;
    padding: 8px 0
  }

  @media print {
    .invoice {
      font-size: 11px !important;
      overflow: hidden !important
    }

    .invoice footer {
      position: absolute;
      bottom: 10px;
      page-break-after: always
    }

    .invoice>div:last-child {
      page-break-before: always
    }
  }
</style>
