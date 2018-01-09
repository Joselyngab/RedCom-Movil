<template>
    <v-ons-page id="app">
       <v-ons-toolbar  v-if="mostrar()" class="toolbar--material">
            <div class="left">
                <router-link to="/principal"><v-ons-back-button style="color: white"></v-ons-back-button></router-link>
            </div>
            <div class="center"  v-if="mostrar()">
               <img :src="this.imagenUrl" style="width: 40px; height:40px; margin-left:0; margin-top:8px;">
            </div>
        </v-ons-toolbar>
    <v-ons-card>

        <img src="../assets/perfil.jpg" alt="Imagen perfil" >
          <div class="title">
            {{user.name}}
          </div>
      <div class="content">

        <v-ons-list>
          <v-ons-list-header>Acerca de {{user.name}}</v-ons-list-header>
          <v-ons-list-item>{{user.userperfil.info}}</v-ons-list-item>
          <v-ons-list-item>Mi Estado actual es: {{user.userperfil.estado}}</v-ons-list-item>

        </v-ons-list>
        <div v-if="show">
           <router-link :to="url"><v-ons-button class="material" style="border-radius:50%;
                                                  position: absolute;
                                                  right: 16px;
                                                  top:8px;"
                                                 >
             <v-ons-icon icon="md-edit" ></v-ons-icon></v-ons-button></router-link>
        </div>
        <div class="boton">
        <v-ons-button  modifier="material" class="button button--light" @click="showModal">{{user.seguidos.length}} Seguidos</v-ons-button>
        <v-ons-modal :visible="modalVisible" @click="modalVisible = false">
            <p style="text-align: center">
                <seguidos-page></seguidos-page>
            </p>
        </v-ons-modal>
        <v-ons-button  modifier="material" class="button button--light"  @click="showModal1">{{user.seguidores.length}} Seguidores</v-ons-button>
        <v-ons-modal :visible="modalVisible1" @click="modalVisible1 = false">
            <p style="text-align: center">
                <seguidores-page></seguidores-page>
            </p>
        </v-ons-modal>
        </div>

      </div>
    </v-ons-card>
          <div class="estado">
            <estado-page></estado-page>
          </div>
    <v-ons-card>
      <v-ons-list-header>Publicaciones</v-ons-list-header>
    <post-page></post-page>
     </v-ons-card>
    </v-ons-page>
</template>

<script>

import Post from './Post.vue'
import Estado from './Estado.vue'
import Seguidores from './Seguidores.vue'
import Seguidos from './Seguidos.vue'
import axios from 'axios'
import auth from '../auth'
export default {
  name: 'perfil',
   components:{
    'post-page': Post,
    'estado-page': Estado,
    'seguidores-page':Seguidores,
    'seguidos-page':Seguidos
   },
   created(){
      var id = this.getParameterByName('id');
      this.getUser();
      this.url = '/editarperfil/?id=' + this.idUser;
      console.log(this.url)
      this.imagenurl = '../assets/img' + this.user.userperfil.avatar
   },
  data(){
    return{
      nombre: 'Andrea',
      descripcion: 'Licenciada en Educación',
      seguidos:'150',
      seguidores:'100',
      modalVisible: false,
      modalVisible1:false,
      idUser:'',
      user: [],
      url: '',
      show: false,
      imagenurl:'',
      imagen:''

    }
  },
  methods: {
    showModal() {
            this.modalVisible = true;

        },
    showModal1(){
      this.modalVisible1= true;
    },
     getParameterByName: function(id, url) {
      if (!url) url = window.location.href;
        id = id.replace(/[\[\]]/g, "\\$&");
        var regex = new RegExp("[?&]" + id + "(=([^&#]*)|&|#|$)"),
            results = regex.exec(url);
        if (!results) return null;
        if (!results[2]) return '';
        this.idUser = decodeURIComponent(results[2].replace(/\+/g, " "));

        console.log(this.idUser);
        return this.idUser
    },
      getUser: function (){
           if (this.idUser == "")
        {
          this.user = auth.getUser();
          this.show = true;
        }else{
            var url = 'http://127.0.0.1:8000/api/user/'+this.idUser+'/?format=json';

              axios.get(url).then(response => {
                this.user = response.data
              });
        }


      },
      mostrar: function(url){
         if (!url) url = window.location.href;

         if(url == "http://localhost:3333/#/principal")
         {
           return false
         }
         else{
           return true
         }
      }
  },
   /*   getUserInstance: function (){
        this.user = auth.getUser()
        this.idUser = this.user.userperfil
        var url = 'http://127.0.0.1:8000/api/perfil/'+this.idUser+'/?format=json';
              axios.get(url,{
                headers: {Authorization: `JWT ${auth.getAuthHeader()}`}
              }).then(response => {
                this.perfil = response.data
              });
        console.log(this.user);
      },
    },*/


}
</script>

<style scoped>

.button--light {
  background-color: transparent;
 color: rgba(0,0,0,0.4);
  border: 1px solid rgba(0,0,0,0.2);
  font-size: 12px;

}
.button--light:active {
  background-color: rgba(0,0,0,0.05);
  color: rgba(0,0,0,0.4);
  border: 1px solid rgba(0,0,0,0.2);
  opacity: 3;

}
img{
     border-radius: 50%;
     width: 150px;
     height: 150px;

     }

.toolbar--material{
    background-color: purple;
}
ons-card {
  text-align: center;

}
</style>
