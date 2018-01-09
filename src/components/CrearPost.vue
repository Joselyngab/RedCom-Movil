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
        <div class="center">
             <h3  style="color: rgb(10, 160, 152);">Nueva Publicación</h3>
        </div>
        <div class="container">
            <form >
             <v-ons-row>
                    <v-ons-col>
                        <v-ons-button id="im" modifier="quiet"  @click="value=true" v-model="value" v-bind="value">
                                <div style="color:#5d6367"><v-ons-icon style="color: #61e4b8; margin-right: 5px; margin-top:10px " icon="md-camera" size="20px"></v-ons-icon>   Camara</div>
                        </v-ons-button>
                    </v-ons-col>
                    <v-ons-col>
                        <v-ons-input type="image"  modifier="quiet" @change="mostar()"  v-model="post.imagen">
                            <div style="color:#5d6367"><v-ons-icon style="color: rgb(172, 7, 187); margin-right: 5px; margin-top:10px " icon="md-collection-image" size="20px"></v-ons-icon> Fototeca</div>
                        </v-ons-input>
                    </v-ons-col>
                     </v-ons-row>
                 <v-ons-card v-if="value">
                 <div class="center">
                    <img id="myimg" :src = 'this.post.imagen' style="width: 80%">
                </div>
                </v-ons-card>
                <br>
                <div class="row l">

                <div class="input-field col s12 m6 l4">
                    <input id="situacion" type="text" class="validate">
         			 <label for="situacion">Cuéntanos qué está sucediendo</label>
                 </div>

                 </div>

                <div class="row">
                <div class="col s12 m12 l6">
                 <div class="input-field">
                     <v-text-area name="contenido" id="contenido" length="50" v-model="post.contenido"></v-text-area>
                         <label for="contenido">Describe la Situación</label>
                 </div>
                </div>
                 </div>
                <v-ons-list>
                     <v-ons-list-item>
                    <div class="center">
                    <label>¿Con qué área se relaciona esta situación?</label>
                      <v-ons-select name="area" material class="material" style="width: 80%" v-model="post.categoria" >
                        <option class="tam3" v-for="item in categ" :value="item.id" :key="item.key">
                            {{ item.nombre }}
                        </option>
                    </v-ons-select>
                    </div>

                    </v-ons-list-item>
                </v-ons-list>

				<div class= "tit">
					<i class="material-icons prefix" style="color: rgb(10, 160, 152);">place</i>
					<h5 style="color: rgb(10, 160, 152);">¿Dónde ocurrió?</h5>
				</div>
                <label>Estado</label>
                 <div class="center">
                <v-ons-select name="edo" id="edo"  v-on:change="getCiudad()" material class="material" style="width: 80%" v-model="post.estado" required>
                        <option class="tam" name="edos" id="edos" v-for="item1 in estados" :value="item1.id" :key="item1.key">
                            {{ item1.estado }}
                        </option>
                    </v-ons-select>

                     </div>
                    <label>Ciudad</label>
                    <div class="center">
                   <v-ons-select name="ciudad" id="ciudad" material class="material" style="width: 80%" v-model="post.ciudad" required >
                        <option default="seleccione" class="tam1" v-for="item2 in ciudad.ciudades" :value="item2" :key="item2.key" >
                            {{item2}}
                        </option>
                    </v-ons-select>
                     </div>
                        <div class="col s12 m12 l6">
                        <div class="input-field">
                            <v-text-area name="contenido" id="contenido" length="50" v-model="post.direccion"></v-text-area>
                            <label for="text">Dirección</label>
                        </div>
                     </div>

                <section style="margin: 20px"></section>
                <div class="center">
                <v-ons-button  modifier="material large" class="button button--light" style="margin: 6px 0" @click="submit()">Publicar</v-ons-button>
                </div>

                    <div id="deviceready" class="blink">
                         <p class="event listening"></p>
                            <p class="event received"></p>
                     </div>

            </form>
        </div>
    </v-ons-page>

</template>

<script>
    import axios from 'axios'
    import auth from '../auth'
export default {
   name: 'crearpost',
  created: function() {
     this.getEstado();
     this.getCat();
    this.getUserCon();
      },
   data : function() {
        return {
            value:{type:Boolean},
            value:false,
            post:{
            titulo:'',
            imagen:{type:File},
            contenido:'',
            categoria:'',
            estado:'',
            ciudad:'' ,
            direccion:'',
            },
        url: '',
        categ:[],
        ciudad: [],                    //arreglo que almacena las ciudades del estado seleccionado
        estados:[],                   //arreglo que almacena los estados
        user:{}
       }

   },
   methods: {
     submit(){
       var post={
         categoria:this.post.categoria,
         img:this.post.imagen,
         tags:[],
         contenido:this.post.contenido,
         activa:true,
         direccion:this.post.direccion,
         autor:this.user.id,
         respaldos:[],
         ciudad:this.post.ciudad,
         likes:null,
         titulo:this.post.titulo,
         estado:this.post.estado,
         comentarios:[]
       }
       auth.createPost(post,'/Principal')
     },
     getUserCon(){
       this.user=auth.getUser();
     },

        getCiudad: function(){
          this.url="http://127.0.0.1:8000/api/estados/"+this.selectedItem+"/?format=json";
           axios.get(this.url).then(response =>{
         this.ciudad = response.data
                });
       },
       getEstado: function(){
       axios.get('http://127.0.0.1:8000/api/estados/?format=json').then(response =>{
         this.estados = response.data
       });

     },
     getCat: function(){
         axios.get('http://localhost:8000/api/categoriapost/?format=json').then(response =>{
         this.categ = response.data
       });
     },
       camara(){
       function cameraTakePicture() {
   navigator.camera.getPicture(onSuccess, onFail, {
      quality: 50,
      destinationType: Camera.DestinationType.DATA_URL
   });

   function onSuccess(imageData) {
      var image = document.getElementById('myimg');
      image.src = "data:image/jpeg;base64," + imageData;
   }

   function onFail(message) {
      alert('Failed because: ' + message);
   }
}},

   }
}
</script>

<style scoped>
.inp{
  margin-top: 10px;
  width: 100%;
}
.tam{
    font-size: 10px;
}

.tit{
	    display: flex;
    align-items: center;
    flex-direction: row;
    margin-bottom: 15px;
}

.ic {
	width: 20%;
    display: flex;
    align-items: center !important;
    flex-direction: row;
    margin-left: 10px;
	margin-right: 0px;
}
.ub{
	width: 100%;
    display: flex;
    align-items: center;
    flex-direction: row;
    margin-left: 0px;
}
.publ{
    margin: 0 auto;
    padding: 0;
    margin-top: 10px;
    margin-bottom: 5px;
}

h6{
  font-size: 15px;
}
.col2{
    width: 100%;
    display: flex;
    flex-direction: row;
    margin-top: 7px;
    margin-left:20px;
}
ons-toolbar{
  color: purple;
}
h3{
    text-align: center;
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
.l{
    margin-top: 30px;
}

.tit{
	display: flex;
    align-items: center;
    flex-direction: row;
    align: center;
    margin-left: 80px;
}
.toolbar--material{
    background-color: purple;
}
</style>
