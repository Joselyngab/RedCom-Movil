<template>
    <v-ons-page id="app">
		<v-ons-toolbar class="toolbar--material">
            <div class="left">
                <router-link to="/"><v-ons-back-button style="color: white"></v-ons-back-button></router-link>
            </div>
            <div class="center" >
               <router-link to="/">
                    <img src='../assets/img/rc1.png' style="width: 40px; height:40px; margin-left:100px; margin-top:8px;">
                </router-link>
            </div>
     </v-ons-toolbar>
      <div class="container">
			<div align="center">
				<h3 style="color: rgb(10, 160, 152);">Registro de Entes</h3>
			</div>
	      <div class="row">
      		<div class="row">
			  	<div class="input-field col s12 m6">
                <i class="material-icons prefix">email</i>
         	 		<input id="email" type="email" v-model="user.email" class="validate" required>
         			 <label for="email">Correo electrónico</label>
                     <div class="al"> <h4 v-if="usersFilter && usersFilter.length">Este usuario ya se encuentra registrado</h4>
                      <h5  v-else>Usuario disponible</h5></div>
        		</div>

        		<div class="input-field col s12 m6">
         			 <i class="material-icons prefix">business</i>
          			<input name="nombre" id="nombre" type="text" class="validate"  required  v-model="user.name">
          			<label  for="nombre">Nombre</label>
       			</div>
       			<div class="input-field col s12 m6">
          			<i class="material-icons prefix">phone</i>
         	 		<input id="tlf" type="number" class="validate" required v-model="user.tlf" >
          			<label for="tlf">Número telefónico</label>
        		</div>
		 		<div class="input-field col s12 m6">
          			<i class="material-icons prefix">lock</i>
          			<input id="password" type="password" class="validate"  required v-model="user.password">
          			<label for="password">Contraseña</label>
        		</div>
			</div>
				<div class= "tit">
					<i class="material-icons prefix" style="color: rgb(10, 160, 152);">place</i>
					<h5 style="color: rgb(10, 160, 152);">Ubicación</h5>
				</div>
                <br>
                     <v-ons-row>
                    <v-ons-col name="a" id="a">
			        <label>Estado</label>
                    <v-ons-select name="edo" id="edo"  v-on:change="getCiudad()" material class="material" style="width: 80%" v-model="user.estado" required>
                        <option class="tam" name="edos" id="edos" v-for="item1 in estados" :value="item1.id" :key="item1.key">
                            {{ item1.estado }}
                        </option>
                    </v-ons-select>
                    </v-ons-col>
                </v-ons-row>
                  <v-ons-col name="b" id="b">

                     <label>Ciudad</label>

                    <v-ons-select name="ciudad" id="ciudad" material class="material" style="width: 80%" v-model="user.ciu" required >
                        <option class="tam1" v-for="item2 in ciudad.ciudades" :value="item2" :key="item2.key" >
                           {{item2}}
                        </option>
                    </v-ons-select>
                    </v-ons-col>
                <div class="col s12 m12 l6">
               		<div class="input-field">
						      <v-text-area name="contenido" id="contenido" length="50" v-model="user.direccion"  required></v-text-area>
					      	<label for="text"><i class="material-icons">pin_drop</i>Dirección</label>
               		 </div>
           		</div>
				   <label>Área a la que se dedica</label>
			        <div class="center">
                    <v-ons-select name="area" material class="material" style="width: 80%" v-model="user.area" >
                        <option class="tam3" v-for="item in categ" :value="item.id" :key="item.key">
                            {{ item.nombre }}
                        </option>
                    </v-ons-select>
                     </div>
			        <br>
                 <v-ons-dialog cancelable :visible.sync="mostrar">
                  <p style="text-align: center">Error al ingresar los datos</p>
                  </v-ons-dialog>
                  <div class="center">
                  <v-ons-button class="button button--light material" modifier="large"  v-on:click="submit()" >REGISTRAR</v-ons-button>
                  </div>
	          </div>
 	  </div>
 </v-ons-page>
</template>
<script>
    import Vue from 'vue';
    import VeeValidate from 'vee-validate';
    import axios from 'axios'
    import auth from '../auth'
    Vue.use(VeeValidate);
    export default {
        name: 'regente',
        created: function() {
         this.getEstado();
         this.getCat();
         this.getUser();

  },
//función que se ejecuta al escribir en el input email
   computed:{
     usersFilter: function(){

       var textSearch = this.user.email;
       var a = this.users.filter(function(el) {
         return el.email.toLowerCase().indexOf(textSearch.toLowerCase()) !== -1;
       });
       console.log('sii');
       if(a && a.length){
         this.valido=true
       }
       return a;

     },
   },
		data : function() {
        return {
           user:{
        _cls: '',
        email: '',
        name: '',
        password: '',
        estado: '',
        ciu: '',
        direccion: '',
        area:'',
        tlf:0,
        },

        categ:[],
        ciudad: [],                    //arreglo que almacena las ciudades del estado seleccionado
        estados:[],                   //arreglo que almacena los estados
        users: [],                    //utilizado para la búsqueda del correo electrónico
        mostrar: false,
        valido:false,
       }

   },
   methods:{
       submit(){
    var crede = {
         username:this.user.email,
         password1:this.user.password,
         password2:this.user.password,
         email:this.user.email,
       }
       var user = {
          direccion:this.user.direccion,
          name: this.user.name,
          modificado: null,
          seguidos: [],
          activo: true,
          slug:this.user.name,
          ciudad:this.user.ciu,
          area:this.user.area,
          telefono:this.user.tlf,
          _cls: "User.Ente",
          seguidores: [],
          userperfil: {
            avatar: "avatar-ente.png",
            info: "Nuevo en REDCOM",
            estado: " Con expectativas"
            },
          password: this.user.password,
          estado: this.user.estado,
          email:this.user.email,
          notificaciones: []
       }
       window.alert(this.valido)
       if(!this.valido){
         this.mostrar=true

       }else{
          auth.signup(crede,'/reg1',user)
       }

     },
        getCiudad: function(){
          this.url="http://127.0.0.1:8000/api/estados/"+this.user.estado+"/?format=json";
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
      //método utilizado para llenar el arreglo de users
           getUser: function(){
               axios.get('http://127.0.0.1:8000/api/user/?format=json')
                 .then(response => {
                   this.users = response.data });
                    },
           }
  }

</script>
<style scooped>
.btn1{
    width: 100%;
    box-sizing: border-box;
    font: inherit;
    font-style: inherit;
    font-variant-ligatures: inherit;
    font-variant-caps: inherit;
    font-variant-numeric: inherit;
    font-variant-east-asian: inherit;
    font-weight: 500;
    font-stretch: inherit;
    font-size: 14px;
    line-height: 36px;
    font-family: 'Roboto', 'Noto', sans-serif;
    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, .14), 0 1px 5px 0 rgba(0, 0, 0, .12), 0 3px 1px -2px rgba(0, 0, 0, .2);
    min-height: 36px;
    line-height: 36px;
    padding: 0 16px;
    text-align: center;
    font-size: 14px;
    text-transform: uppercase;
    font-weight: 500;
    opacity: 1;
    line-height: 32px;
    letter-spacing: 0;
    border-radius: 3px;
    -webkit-font-smoothing: antialiased;

}
.tit{
	    display: flex;
    align-items: center;
    flex-direction: row;
    align: center;
    margin-left: 100px;
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
    margin-left: 10px;
}

h5{
	text-decoration: underline;

}
.row1{
	width: 100%;
    display: flex;
    align-items: center;
    flex-direction: row;
    margin-left: 0px;
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
.toolbar--material{
    background-color: purple
}
h4{
    font-size: 12px;
    color: red;
    line-height: 0px;

}
h5{
    font-size: 12px;
    color: #26a69a;
    line-height: 0px;

}
.al{
    margin-left: 45px;
    margin-top: 0px;
}

</style>
