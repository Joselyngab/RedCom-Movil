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
				<h3 style="color: rgb(10, 160, 152);">Registro de Personas</h3>
			</div>
	<div class="row">
    <div class="col s12">
      		<div class="row">
				  <div class="input-field col s12 m6">
         			 <i class="material-icons prefix">email</i>
         	 		<input id="email" type="email" v-model="user.email" class="validate" required>
                <label for="email">Correo electrónico</label>
                  <div v-show="show">
                  <div class="al"> <h4 v-if="usersFilter && usersFilter.length">Este usuario ya se encuentra registrado</h4>
                      <h5  v-else>Usuario disponible</h5></div>
                  </div>

        		</div>
        		<div class="input-field col s12 m6">
         			 <i class="material-icons prefix">account_circle</i>
          			<input id="nombre"  type="text" class="validate" required v-model="user.name" >
          			<label for="nombre">Nombre</label>
       			</div>
       			<div class="input-field col s12 m6">
          			<i class="material-icons prefix">account_circle</i>
         	 		<input id="apellido" type="text" class="validate" required v-model="user.apellido">
          			<label for="apellido">Apellido</label>
        		</div>
		 		<div class="input-field col s12 m6">
          			<i class="material-icons prefix">lock</i>
          			<input id="password" type="password" class="validate" required v-model="user.password">
          			<label for="password">Contraseña</label>
        		</div>
		    	</div>
                <label>Sexo</label>
                <div class="center">
                    <v-ons-select name="sexo" material class="material" style="width: 80%" v-model="user.sexo" required>
                        <option class="tam" v-for="item2 in sexo" :value="item2.value" :key="item2.key">
                            {{ item2.text }}
                        </option>
                    </v-ons-select>
                </div>
                <br>
			            <v-ons-row>
                    <v-ons-col>
			              <label>Estado</label>
                    <v-ons-select dafault="Seleccione una Opcion" name="edo" id="edo"  v-on:change="getCiudad()" material class="material" style="width: 80%" v-model="user.estado" required>
                        <option class="tam" name="edos" id="edos" v-for="item1 in estados" :value="item1.id" :key="item1.key">
                            {{ item1.estado }}
                        </option>
                    </v-ons-select>
                    </v-ons-col>
                </v-ons-row>
	  	          <br>
           <v-ons-col name="b" id="b">
                     <label>Ciudad</label>
                    <v-ons-select name="ciudad" id="ciudad" material class="material" style="width: 80%"  required v-model="user.ciu">
                        <option class="tam1" v-for="item2 in ciudad.ciudades" :value="item2" :key="item2.key">
                            {{item2}}
                        </option>
                    </v-ons-select>
                    </v-ons-col>
                <div class="col s12 m12 l6">
               <div class="input-field">
                     <v-text-area name="contenido" id="contenido" length="50" v-model="user.direccion" required></v-text-area>
                     <label for="text"><i class="material-icons">pin_drop</i>Dirección</label>
                </div>
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
	</div>
 </v-ons-page>
</template>
<script>
import {mapGetters} from 'vuex';
  import axios from 'axios';
  import auth from '../auth'
export default {
	name: 'registro',
    created: function() {
     this.getEstado();
    this.getUser();
    },
   //función que se ejecuta al escribir en el input email
   computed:{
     usersFilter: function(){
         var inpEm=$("#email").val();
       if( this.user.email == "")
       { this.show = false}
       else{this.show = true}
       var textSearch = this.user.email;
       var a = this.users.filter(function(el) {
         return el.email.toLowerCase().indexOf(textSearch.toLowerCase()) !== -1;
       });
       if(a && a.length){
         this.valido=true
       }
       return a;
     }
   },
	data : function() {
        return {
           user:{
            email: '',
            name: '',
            apellido:'',
            password: '',
            estado: '',
            ciu: '',
            direccion: '',
            sexo:0
        },
      sexo: [
        { text: 'Femenino', value: 0 },
        { text: 'Masculino', value: 1 },
        ],
        users: [],                    //utilizado para la búsqueda del correo electrónico
        show: false,
        mostrar: false,
        ciudad: [],                    //arreglo que almacena las ciudades del estado seleccionado
        estados:[],
        valido:true                   //arreglo que almacena los estados
	}},
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
          sexo:this.user.sexo,
          _cls: "User.Persona",
          seguidores: [],
          userperfil: {
            avatar: "avatar-persona.png",
            info: "Nuevo en REDCOM",
            estado: " Con expectativas"
            },
          password: this.user.password,
          estado: this.user.estado,
          email:this.user.email,
          notificaciones: []
       }
       if(this.user.email !==""){
         auth.signup(crede,'/reg1',user)
       }else{
           this.mostrar=true
       }

     },
        //método utilizado para llenar el arreglo de users
     getUser: function(){
          axios.get('http://127.0.0.1:8000/api/user/?format=json')
        .then(response => {
        this.users = response.data
    });
     },
     //método para buscar llenar el array ciudad con los datos del estado seleccionado
       getCiudad: function(){
          this.url='http://127.0.0.1:8000/api/estados/'+this.user.estado+'/?format=json';
           axios.get(this.url).then(response =>{
         this.ciudad = response.data

                });

       },
       //método que se ejecuta en created para llenar el arreglo estados con los estados de venezuela
      getEstado: function(){
          console.log('099')
       axios.get('http://127.0.0.1:8000/api/estados/?format=json').then(response =>{
         this.estados = response.data
       });
     },

    },

};
</script>
<style scoped>
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
p{
	color: black;
	font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
	font: Roboto;
	font-size: 20px;
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
	background-color: purple;
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
