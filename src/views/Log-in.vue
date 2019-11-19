<template>
<div align="center" class="myStyle">
  <section class="form-elegant">
      <mdb-col md="5">
        <mdb-card class="myCard">
          <mdb-card-body class="mx-4">
            <div class="text-center">
              <h3 class="titleLogin">Iniciar sesión</h3>
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
          <mdb-modal-footer class="mx-5 pt-3 mb-1" align="rigth">
           <p class="littleFont">¿No está registrado? <div class="my-2"><v-btn text small @click="abrirRegistro()" color="#f90">Registrar empleado</v-btn></p></div>
          </mdb-modal-footer>
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
                 
         console.log(this.username);
         axios.get(`https://ctmbackend.herokuapp.com/api/users/${this.username}`).then(result => {
          console.log(result.data);
          if(result.data[0].password != this.password && result.data[0].nombreUsuario != this.username){
            alert("Nombre de usuario o usuario Incorrecto");
          }
          else{
            this.principal();
          }
            
        }).catch((error) => {
          console.error(error)
          alert("Nombre de usuario o usuario incorrecto");
        })
      },
     /* solicitarAcceso(token,id){
            fetch(`https://ctmbackend.herokuapp.com/api/users/${id}`, {
            method: "GET",
            headers:{
              "Content-Type": "application/json",
              "Authorization": `Bearer ${token}`
            }
          })
          .then(response => {
          })
          .catch(error => {
          	alert("El usuario o contraseña son incorrectas")
          });
      }*/
    }
  }
</script>

<style>
  .myStyle {
  margin-top: 80px;
  }
  .myCard {
    background: white;
  }
  .titleLogin{
    color:black;
  }
  .littleFont{
    color:#ff4b0d;
  }
  .footer{
    margin-right:-400px;
  }
</style>
