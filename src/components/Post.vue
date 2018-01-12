<template>
<div class="container" id="app">
     <v-ons-list>
      <v-ons-list-item v-for="publ in publicacion" :key="publ.link">
        <v-ons-card >
            <div class="pub">
               <div class="img">
                   <img src="../assets/perfil.jpg" style="width: 150% !important;" class="perfil">
               </div>
                <div class="col">
                 <div class="f1">
                     <!--  <h4>{{p.authenticated}}</h4>-->
                     <h4>{{buscarUser(publ.autor)}}</h4>

                  </div>
                  <div class="f2">

                      <h6>Hace 2 minutos</h6>
                  </div>
                </div>
            </div>
            <div v-on:click="getDetalle(publ.id)">
                <router-link :to="url">
                   <h6>Ver detalle de la publicación</h6>
                </router-link>
            </div>
                    <div class="card__imagen posti" >
                    </div>
                    <div class="card_content"  >
                      <p style="color: black;">{{publ.contenido}}</p>
                    </div>

            <div class="card_content">
            </div>
            <v-ons-row>
                <v-ons-col>
                    <v-ons-button modifier="quiet material" style="color: #5d6367">
                         {{publ.likes}}                      <v-ons-icon icon="md-thumb-up" size="20px"></v-ons-icon>
                    </v-ons-button>
                </v-ons-col>
                <v-ons-col>
                  <router-link to="/comentario">
                     <v-ons-button modifier="quiet material" style="color: #5d6367">
                         <v-ons-icon icon="md-comments" size="20px"></v-ons-icon>
                    </v-ons-button>
                  </router-link>
                </v-ons-col>
                <v-ons-col>
                   <v-ons-button modifier="quiet material" style="color: #5d6367">
                         <v-ons-icon icon="md-label" size="20px"></v-ons-icon>
                    </v-ons-button>
                </v-ons-col>
            </v-ons-row>
            <div class="com">

            </div>
        </v-ons-card>
      </v-ons-list-item>
    </v-ons-list>
</div>
</template>
<script>
import auth from '../auth'
import img2 from '../assets/img/falla.jpg'
import DetallePost from './DetallePost.vue'
import Comentarios from './Comentario.vue'
import axios from 'axios'
export default {
  name: 'post',
   beforeCreated: function() {
     this.getPublicac();
     this.getUser1();
     this.postMuro();
  },

  components: {
    DetallePost,
    'comentarios-page': Comentarios,
  },


  data: function() {
    return {

        img:{
            type: File,
          },
          img:'',

          publicacion:[],
          user: [],
           user1: [],
          categoria: [],
          autor:'',
          url: '',
          muro: [],
          us:{},
      }
  },
  methods: {
    getPub: function(){

         this.publicacion= auth.checkTipoUser();
        },
     },
     getUser1: function(){
        this.user1=auth.getUser();
       },

     postMuro:function(){
       console.log(this.user1.name)
       if(this.user1.seguidos.length == 0)
         {
           for (i=0; i<this.publicacion.length; i++){
            if(this.publicacion[i].autor == this.user1.id)
                this.muro.push(this.publicacion[i])
            }
         }
         else{
           for (i=0; i<this.publicacion.length; i++){
             for (j=0; j<this.user1.seguidos.length; j++){
                if((this.publicacion[i].autor == this.user1.seguidos[j]) || (this.publicacion[i].autor == this.user1.id))
                    this.muro.push(this.publicacion[i])
                }
             }

         }
         console.log(this.muro)
     },
       getDetalle: function(id){

        this.url = '/detallepost/?id='+id;
        console.log(this.url)
    },





};
</script>
<style>
.texto{
   display: flex;
    align-items: center;
    width: 100%;
    justify-content: flex-end;
    margin-left: 0px;

}
h6{
  font-size: 15px;
}
.ic{
  width: 20%;
  display: flex;
  margin-right:5px;
}

.p{
    display: inline-block;

}
.perfil {
    border-radius: 50%;
}
.pub{
    display: flex;
    align-items: center;
    flex-direction: row;
    width: 100%;
}

.img{
    display: flex;
    align-items: center;
    flex-direction: row;
    width: 30px;
}
.posti{
  margin-top: 15px;
}
.col{
  display: flex;
    align-items: left;
    flex-direction: column;
    width: 100%;
    margin-left: 20px;
}
.botn{
  display: flex;
    align-items: center;
    flex-direction: column;
    width: 100%;
}
.gustar{
  margin: 0 auto;
  width: 50%;
  display: flex;
  flex-direction: column;
  margin-left: 5px;
	margin-right: 5px;
}
.col2{
    width: 100%;
    display: flex;
    flex-direction: row;
    margin-top: 7px;
}
h4{
  line-height: 0;
}

h6{
  line-height: 0;
  font-size: 10px;
}


</style>
