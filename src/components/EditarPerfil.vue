<template>
    <v-ons-page id="app">
        <v-ons-toolbar class="toolbar--material">
            <div class="left">
                <router-link to="/perfil"><v-ons-back-button style="color: white"></v-ons-back-button></router-link>
            </div>
            <div class="center" >
               <img src='../assets/img/rc1.png' style="width: 40px; height:40px; margin-left:0; margin-top:8px;">
            </div>
        </v-ons-toolbar>
        <div class="center">
				<h3  style="color: rgb(10, 160, 152);">Editar perfil</h3>
        </div>
   		<form >
                <v-ons-row>
                        <v-ons-col>
                             <v-ons-button id="im" modifier="quiet" @click="camara()" v-bind="ver" v-model="ver">
                                 <div style="color:#5d6367"><v-ons-icon style="color: #61e4b8; margin-right: 5px; margin-top:10px " icon="md-camera" size="20px"></v-ons-icon>  Camara</div>
                             </v-ons-button>
                        </v-ons-col>
                        <v-ons-col>
                            <v-ons-button id="im" modifier="quiet"  @click="ver=true" v-model="ver" v-bind="ver">
                                <div style="color:#5d6367"><v-ons-icon style="color: rgb(172, 7, 187); margin-right: 5px; margin-top:10px " icon="md-collection-image" size="20px"></v-ons-icon> Fototeca</div>
                            </v-ons-button>
                        </v-ons-col>
                </v-ons-row>
            <br>
           <v-ons-card v-if="ver">
                 <div class="center">
                     <img id="myimg" style="width: 80%">
                 </div>
            </v-ons-card>
            <br>
            <div class="container">
            <div class="row">
                <form class="col s12" action="#">
                    <div class="row">
                          <div class="input-field col s12 m6">
                             <i class="material-icons prefix">email</i>
                            <input id="correo" type="text" class="validate" :value="user.email" disabled v-model="user.email">

                        </div>
                        <div class="input-field col s12 m6">

                            <i class="material-icons prefix">account_circle</i>
                            <input id="nombre" type="text" class="validate" :value="user.name" v-model="user.name">
                            <label for="nombre" >Nombre</label>
                        </div>
                          <div class="input-field col s12 m6">
                            <i class="material-icons prefix">sentiment_very_satisfied</i>
                            <input id="descripcion" type="text" class="validate" :value="user.userperfil.estado" v-model="actual">
                            <label for="descripcion" >Como estas Ahora</label>
                        </div>
                        <div class="input-field col s12 m6">
                            <i class="material-icons prefix">sentiment_very_satisfied</i>
                            <input id="descripcion" type="text" class="validate" :value="user.userperfil.info" v-model="informacion">
                            <label for="descripcion" >Sobre mí</label>
                        </div>
                        <div class="input-field col s12 m6">
                            <i class="material-icons prefix"> location_on</i>
                            <input id="direccion" type="text" class="validate" :value="user.direccion" v-model="user.direccion">
                            <label for="direccion" >Dirección</label>
                        </div>

                    </div>
                </form>
            </div>
          </div>
          <v-ons-dialog cancelable :visible.sync="visible">
                    <p style="text-align: center">Error al ingresar los datos</p>
                   </v-ons-dialog>
            <section style="margin: 20px">
                <v-ons-button class="button button--light" modifier="material large" @click="submit()" >Guardar</v-ons-button>

            </section>
        </form>
    <div id="deviceready" class="blink">
        <p class="event listening"></p>
        <p class="event received"></p>
    </div>

    </v-ons-page>


</template>
<script>
import axios from 'axios'
import auth from '../auth'
 export default {
      created(){
      this.Conectado();
   },
     name: 'editarperfil',

     data(){

      return{
          ver:{type:Boolean, default:false},
          ver:false,
            idUser:'',
            user:{
              id:'',
              name:'',
              email:'',
              userperfil:{
              avatar:'',
              info:'',
              estado: '',
            },
            direccion:''
            },
             estados:[],                   //arreglo que almacena los estados
            show: false,
            visible:false,
            informacion:'',
            actual:'',
    }
  },



   methods: {
     Conectado(){
       this.user = auth.getUser()
     },
     submit(){
          var perfil={
            id:this.user.id,
            name: this.user.name,
            email:this.user.email,
            userperfil:{
              avatar: this.user.userperfil.avatar,
              info: this.informacion,
              estado:this.actual
            },
            direccion:this.user.direccion

          }
          if(this.user.name!=="" && this.user.direccion!="" && this.actual!=""){
            auth.updatePerfil(perfil, '/principal')
          }
          else{
            this.visible = true
          }
      },


            }
 }
</script>
<style scoped>
.button--light {
  background-color: transparent;
  color: rgba(0,0,0,0.4);
  border: 1px solid rgba(0,0,0,0.2);

}
.button--light:active {
  background-color: rgba(0,0,0,0.05);
  color: rgba(0,0,0,0.4);
  border: 1px solid rgba(0,0,0,0.2);
  opacity: 3;

}

.toolbar--material{
    background-color: purple;
}

ons-card {
  text-align: center;

}

.ic {
	width: 20%;
    display: flex;
    align-items: center !important;
    flex-direction: row;
    margin-left: 10px;
	margin-right: 0px;
}

h6{
  font-size: 15px;
}



</style>
