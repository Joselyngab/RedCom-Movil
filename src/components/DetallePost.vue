<template>
<v-ons-page id="app">
    <v-ons-toolbar class="toolbar--material">
            <div class="left">
                <router-link to="/principal"><v-ons-back-button style="color: white"></v-ons-back-button></router-link>
            </div>
            <div class="center" >
               <img src='../assets/img/rc1.png' style="width: 40px; height:40px; margin-left:0; margin-top:8px;">
            </div>


        </v-ons-toolbar>
    <v-ons-card>
    <div class="pub">
       <div class="img">
           <img src="../assets/perfil.jpg" style="width: 150% !important;" class="perfil">
       </div>
       <div class="col">
           <div class="f1">
                <h4>Andrea</h4>
            </div>
            <div class="f2">
                <h6>Hace 2 minutos</h6>
            </div>
        </div>
    </div>

    <div class="card_content">
        <h4 style="color: black" ></h4>
        <h4>{{buscarCat(publicacion.categoria)}}</h4>
        <h5 style="color: blue">En Edo. {{buscarEstado(publicacion.estado)}}</h5>


    </div>
    <div class="card__imagen posti">
        <img src="../assets/img/falla.jpg" style="width: 100%">
    </div>
    <div class="center">

        <div class="card_content">
            <h3 style="font-weight: bold;">{{publicacion.titulo}}</h3>

        </div><div class="algo">
         <h4>{{publicacion.contenido}}</h4></div>
    </div>

         <div class="center">
                <router-link to="/comentario" ><v-ons-button  modifier="material quiet"  style="margin: 6px 0; color: #5d6367"><v-ons-icon icon="md-comments" size="30px"></v-ons-icon></v-ons-button></router-link>
                <v-ons-button  modifier="quiet material" style="color: #5d6367" ><v-ons-icon icon="md-label" size="30px"></v-ons-icon></v-ons-button>
         </div>

</v-ons-card>

</v-ons-page>
</template>

<script>
import img2 from '../assets/amigos.jpg'
import axios from 'axios'
export default {
     name: 'detallepost',
    created: function() {
         var id = this.getParameterByName('id');
         this.getPublicac();
      },
     data () {
         return {
               publicacion:[],
               idPub: '',
               categoria:[],
               estado:[],
               ciudad:[],

        }

    } ,
             methods: {

     getUrl: function(algo){
       var dir = algo;
       var url = "../assets/img/" + dir;
       return url;
     },
      getParameterByName: function(id, url) {
      if (!url) url = window.location.href;
        id = id.replace(/[\[\]]/g, "\\$&");
        var regex = new RegExp("[?&]" + id + "(=([^&#]*)|&|#|$)"),
            results = regex.exec(url);
        if (!results) return null;
        if (!results[2]) return '';
        this.idPub = decodeURIComponent(results[2].replace(/\+/g, " "));

        return this.idPub
    },
     getPublicac: function(){

       axios.get('http://127.0.0.1:8000/api/publicacion/'+this.idPub+'/?format=json').then(response =>{
         this.publicacion = response.data
       });
     },
      buscarEstado: function (idE){
       axios.get('http://127.0.0.1:8000/api/estados/'+idE+'/?format=json').then(response =>{
         this.estado = response.data
       });
       console.log(this.estado.estado)
       return this.estado.estado
      },
       buscarCat: function (idC){
       axios.get('http://127.0.0.1:8000/api/categoriapost/'+idC+'/?format=json').then(response =>{
         this.categoria = response.data
       });
       console.log(this.categoria.nombre)
       return this.categoria.nombre
      },
      buscarCiudad: function(i){
          var a = this.estado.ciudades[i]
          console.log(a)
          return a
      }
  },
}


</script>

<style scoped>


.button--light:active {
  background-color: rgba(0,0,0,0.05);
  color: rgba(0,0,0,0.4);
  border: 1px solid rgba(0,0,0,0.2);
  opacity: 3;

}
h3{
    line-height: 15px;
}
h5{
    font-size: 10px;
}
.algo{
    margin-top: 30px;
}
h4{
    font-size: 13px;
    line-height: 15px;
}
.card-image{
    max-width: 10000px;
    margin: 0px, 0px ,0px,0px
}

.img{
    display: flex;
    align-items: center;
    flex-direction: row;
    width: 30px;
}

.col{
  display: flex;
    align-items: left;
    flex-direction: column;
    width: 100%;
    margin-left: 20px;
}
h5{
  text-align: justify;
  font-size: 15px;
  color: #00000099;
  text-decoration: none;
}
.posti{
  margin-top: 15px;
}

.button--light {
  background-color: transparent;
  color: #9E9898;
  border: 1px solid rgba(0,0,0,0.2);
}
.button--light:active {
  background-color: rgba(0,0,0,0.05);
  color: #9E9898;
  border: 1px solid rgba(0,0,0,0.2);
  opacity: 3;

}
h4{
  line-height: 0;
}

h6{
  line-height: 0;
  font-size: 10px;
}

.pub{
    display: flex;
    align-items: center;
    flex-direction: row;
    width: 100%;
}
.toolbar--material{
    background-color: purple;
}
</style>

