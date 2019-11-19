<template>
<div align="center" class="myStyle">
  <section class="form-elegant">
      <mdb-col md="5">
        <mdb-card class="myCard">
          <mdb-card-body class="mx-4">
            <div class="text-center">
              <h3 class="title">Iniciar sesión</h3>
            </div>
            <v-text-field
              v-model="username"
              ref="username"
              label="Ingrese el nombre de usuario"
              required
            ></v-text-field>
            <v-text-field
              v-model="password"
              ref="password"
              label="Ingrese la contraseña"
              type="password"
              required
            ></v-text-field>
            <div>
             <div class="my-2" align="center"> <v-btn text small color="#f90" @click="agregarUsuario()">Ingresar</v-btn></div>
            </div>
          </mdb-card-body>
          <div class="footer" >
           <p class="littleFont">¿No está registrado? <div class="my-2"><v-btn text small @click="abrirRegistro()" color="#f90">Registrar empleado</v-btn></p></div>
          </div>
        </mdb-card>
      </mdb-col>
    </mdb-row>
  </section>
    <agregarRegistro
      :mostrar="mostrarRegistro"
      @ok="mostrarRegistro = false"
      @cancel="mostrarRegistro = false"
    >
    </agregarRegistro>
</div>
</template>

<script>
  import { mdbRow, mdbCol, mdbCard, mdbCardBody, mdbInput, mdbBtn, mdbIcon } from 'mdbvue';
  import agregarRegistro from "./agregarRegistro";
  import axios from 'axios'
  import Qs from 'qs'
  export default {
    name: 'FormsPage',
    components: {
      mdbRow,
      mdbCol,
      mdbCard,
      mdbCardBody,
      mdbInput,
      mdbBtn,
      mdbIcon,
      agregarRegistro
    },
    data() {
      return{
        mostrarRegistro: false,
        token: '',
        username: '',
        password: '',
        id: ''
      }
    },
    methods: {
      principal(){
        this.$router.push({name: "home"})
      },
      abrirRegistro(){
        this.mostrarRegistro=true;
      },
      agregarUsuario(){
          axios.post('https://ctmbackend.herokuapp.com/api/login', Qs.stringify({username: this.username,password: this.password})).then((res) => {
          this.token = res.data.token;
          console.log(res);
          this.id = res.data.user;
          this.solicitarAcceso(this.token,this.id);
          }).catch((error) => {
          })
      },
      solicitarAcceso(token,id){
            fetch(`https://ctmbackend.herokuapp.com/api/users/${id}`, {
            method: "GET",
            headers:{
              "Content-Type": "application/json",
              "Authorization": `Bearer ${token}`,
              "Access-Control-Allow-Origin": "*"
            }
          })
          .then(response => {
            this.principal();
          })
          .catch(error => {
          	alert("El usuario o contraseña son incorrectas")
          });
      }
    }
  }
</script>

<style>
  .myStyle {
  margin-top: 80px;
  },
  .myCard {
    background: white;
  },
  .title{
    color:black;
  },
  .littleFont{
    color:#ff4b0d;
  }
  .footer{

    margin-right:-400px;
  }
</style>
