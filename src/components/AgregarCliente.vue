<template>
    <div class="container register">
        <div class="row">
        <div class="col-md-3 register-left">
          <img src="dist/ferreteria.jpg" style="width: 100%;" alt="" />
          <h3>Reto Factura</h3>
        </div>
        <div class="col-md-9 register-right">
          <div class="tab-content" id="myTabContent">
            <div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab">
              <h3 class="register-heading">Agregar cliente</h3>
              <div class="row register-form">
                <div class="col-md-6">
                  <div class="form-group">
                      <label for="usr">* Nombre:</label>
                    <input type="text" v-model="cliente.nombre" class="form-control" value="" />
                  </div>
                  <div class="form-group">
                      <label for="usr">* Apellido:</label>
                    <input type="text" v-model="cliente.apellido" class="form-control"  value="" />
                  </div>
                  <div class="form-group">
                      <label for="usr">* Identificación:</label>
                    <input type="text" v-model="cliente.identificacion" class="form-control" value="" />
                  </div>
                </div>
                <div class="col-md-6">
                  <div class="form-group">
                      <label for="usr">* Correo:</label>
                    <input type="email" v-model="cliente.correo" class="form-control" value="" />
                  </div>
                  <div class="form-group">
                      <label for="usr">* Celular:</label>
                    <input type="number" v-model="cliente.celular" minlength="10" maxlength="10" name="txtEmpPhone" class="form-control"
                      value="" />
                  </div>
                  <input type="submit" class="btnRegister" value="Register" @click="guardarCliente()" />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
  const encontrarIdentificacion = ({identificacion}, identificacionBusqueda) => identificacion === identificacionBusqueda;
  export default {
    name:"AgregarCliente",
    props: { listaClientes: Array },
    data: function () {
      return {
        cliente: {
          nombre: '',
          apellido: '',
          correo: '',
          identificacion: '',
          celular: ''
        }
      }
    },
    methods: {
      guardarCliente: function () {
        if (this.validarCampos() && this.validarCedula()) {
          this.listaClientes.push(Object.assign({}, this.cliente));
          this.limpiarCampos();
          alert("Cliente guardado con éxito!");
        }
        else alert('Recuerde no se puede registrar la misma identificación y todos los datos son obligatorios.')
      },
      validarCampos: function () {
        for (var key in this.cliente) {
          if (this.cliente[key] === '') return false;
        }
        return true;
      },
      validarCedula: function () {
        return this.listaClientes.filter(clienteEvaluar => encontrarIdentificacion(clienteEvaluar, this.cliente.identificacion)).length === 0;
      },
      limpiarCampos: function () {
        for (var key in this.cliente) {
          this.cliente[key] = '';
        }
      }
    }
  }
</script>

<style>
  .centrar {
    text-align: center;
  }

  /* Estilos agregar cliente*/
  .register {
    background: -webkit-linear-gradient(left, #3931af, #00c6ff);
    margin-top: 3%;
    padding: 3%;
  }

  .register-left {
    text-align: center;
    color: #fff;
    margin-top: 4%;
  }

  .register-left input {
    border: none;
    border-radius: 1.5rem;
    padding: 2%;
    width: 60%;
    background: #f8f9fa;
    font-weight: bold;
    color: #383d41;
    margin-top: 30%;
    margin-bottom: 3%;
    cursor: pointer;
  }

  .register-right {
    background: #f8f9fa;
    border-top-left-radius: 10% 50%;
    border-bottom-left-radius: 10% 50%;
  }

  .register-left img {
    margin-top: 15%;
    margin-bottom: 5%;
    width: 25%;
    -webkit-animation: mover 2s infinite alternate;
    animation: mover 1s infinite alternate;
  }

  @-webkit-keyframes mover {
    0% {
      transform: translateY(0);
    }

    100% {
      transform: translateY(-20px);
    }
  }

  @keyframes mover {
    0% {
      transform: translateY(0);
    }

    100% {
      transform: translateY(-20px);
    }
  }

  .register-left p {
    font-weight: lighter;
    padding: 12%;
    margin-top: -9%;
  }

  .register .register-form {
    padding: 10%;
    margin-top: 10%;
  }

  .btnRegister {
    float: right;
    margin-top: 10%;
    border: none;
    border-radius: 1.5rem;
    padding: 2%;
    background: #0062cc;
    color: #fff;
    font-weight: 600;
    width: 50%;
    cursor: pointer;
  }

  .register .nav-tabs {
    margin-top: 3%;
    border: none;
    background: #0062cc;
    border-radius: 1.5rem;
    width: 28%;
    float: right;
  }

  .register .nav-tabs .nav-link {
    padding: 2%;
    height: 34px;
    font-weight: 600;
    color: #fff;
    border-top-right-radius: 1.5rem;
    border-bottom-right-radius: 1.5rem;
  }

  .register .nav-tabs .nav-link:hover {
    border: none;
  }

  .register .nav-tabs .nav-link.active {
    width: 100px;
    color: #0062cc;
    border: 2px solid #0062cc;
    border-top-left-radius: 1.5rem;
    border-bottom-left-radius: 1.5rem;
  }

  .register-heading {
    text-align: center;
    margin-top: 8%;
    margin-bottom: -15%;
    color: #495057;
  }

  .nav-side-menu {
    overflow: auto;
    font-family: verdana;
    font-size: 12px;
    font-weight: 200;
    background-color: #2e353d;
    position: fixed;
    top: 0px;
    width: 300px;
    height: 100%;
    color: #e1ffff;
  }

  .nav-side-menu .brand {
    background-color: #23282e;
    line-height: 50px;
    display: block;
    text-align: center;
    font-size: 14px;
  }

  .nav-side-menu .toggle-btn {
    display: none;
  }

  .nav-side-menu ul,
  .nav-side-menu li {
    list-style: none;
    padding: 0px;
    margin: 0px;
    line-height: 35px;
    cursor: pointer;
    /*
      .collapsed{
         .arrow:before{
                   font-family: FontAwesome;
                   content: "\f053";
                   display: inline-block;
                   padding-left:10px;
                   padding-right: 10px;
                   vertical-align: middle;
                   float:right;
              }
       }
  */
  }

  .nav-side-menu ul :not(collapsed) .arrow:before,
  .nav-side-menu li :not(collapsed) .arrow:before {
    font-family: FontAwesome;
    content: "\f078";
    display: inline-block;
    padding-left: 10px;
    padding-right: 10px;
    vertical-align: middle;
    float: right;
  }

  .nav-side-menu ul .active,
  .nav-side-menu li .active {
    border-left: 3px solid #d19b3d;
    background-color: #4f5b69;
  }

  .nav-side-menu ul .sub-menu li.active,
  .nav-side-menu li .sub-menu li.active {
    color: #d19b3d;
  }

  .nav-side-menu ul .sub-menu li.active a,
  .nav-side-menu li .sub-menu li.active a {
    color: #d19b3d;
  }

  .nav-side-menu ul .sub-menu li,
  .nav-side-menu li .sub-menu li {
    background-color: #181c20;
    border: none;
    line-height: 28px;
    border-bottom: 1px solid #23282e;
    margin-left: 0px;
  }

  .nav-side-menu ul .sub-menu li:hover,
  .nav-side-menu li .sub-menu li:hover {
    background-color: #020203;
  }

  .nav-side-menu ul .sub-menu li:before,
  .nav-side-menu li .sub-menu li:before {
    font-family: FontAwesome;
    content: "\f105";
    display: inline-block;
    padding-left: 10px;
    padding-right: 10px;
    vertical-align: middle;
  }

  .nav-side-menu li {
    padding-left: 0px;
    border-left: 3px solid #2e353d;
    border-bottom: 1px solid #23282e;
  }

  .nav-side-menu li a {
    text-decoration: none;
    color: #e1ffff;
  }

  .nav-side-menu li a i {
    padding-left: 10px;
    width: 20px;
    padding-right: 20px;
  }

  .nav-side-menu li:hover {
    border-left: 3px solid #d19b3d;
    background-color: #4f5b69;
    -webkit-transition: all 1s ease;
    -moz-transition: all 1s ease;
    -o-transition: all 1s ease;
    -ms-transition: all 1s ease;
    transition: all 1s ease;
  }

  @media (max-width: 767px) {
    .nav-side-menu {
      position: relative;
      width: 100%;
      margin-bottom: 10px;
    }

    .nav-side-menu .toggle-btn {
      display: block;
      cursor: pointer;
      position: absolute;
      right: 10px;
      top: 10px;
      z-index: 10 !important;
      padding: 3px;
      background-color: #ffffff;
      color: #000;
      width: 40px;
      text-align: center;
    }

    .brand {
      text-align: left !important;
      font-size: 22px;
      padding-left: 20px;
      line-height: 50px !important;
    }
  }

  @media (min-width: 767px) {
    .nav-side-menu .menu-list .menu-content {
      display: block;
    }
  }

  body {
    margin: 0px;
    padding: 0px;
  }
</style>
