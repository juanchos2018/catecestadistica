<template>
  <div>
	<a-card :bordered="false" class="header-solid h-full" :bodyStyle="{padding: 4,}">
    <a-form @submit.prevent="Validate">
      <a-row :gutter="[24, 24]">
        <a-col :span="24" :md="6">
          <a-form-item label="Nombres">
            <a-input placeholder="Nombres" v-model="modelUsuario.nombre">
            </a-input>
          </a-form-item>
       
        </a-col>

        <a-col :span="24" :md="6">
          <a-form-item label="Apellidos">
            <a-input placeholder="Apellidos" v-model="modelUsuario.apellidos">
            </a-input>
          </a-form-item>
        </a-col>
        
        <a-col :span="24" :md="6">
          <a-form-item label="Correo">
            <a-input placeholder="correo" v-model="modelUsuario.correo">
            </a-input>
          </a-form-item>
        </a-col>

         <a-col :span="24" :md="6">
          <a-form-item label="Link">
            <a-input placeholder="Link" v-model="modelUsuario.link">
            </a-input>
          </a-form-item>
        </a-col>  
      </a-row>

      <a-row :gutter="[24, 24]">

         <a-col :span="24" :md="6">
          <a-form-item label="Tema">
            <a-input placeholder="tema" v-model="modelUsuario.tema">
            </a-input>
          </a-form-item>
        </a-col>

        <a-col :span="24" :md="6">
          <a-form-item label="Hora inicio">
            <a-time-picker :default-value="moment('12:08', 'HH:mm')" format="HH:mm"  @change="onChange"  />

          </a-form-item>
        </a-col>

       <a-col :span="24" :md="6">
          <a-form-item label="Hora Fin">
            <a-time-picker :default-value="moment('12:08', 'HH:mm')" format="HH:mm"  @change="onChange1"  />

          </a-form-item>
        </a-col>
  

        <a-col :span="24" :md="3">
          <a-form-item label="Registrar">
            <a-button type="primary" html-type="submit" >
              REGISTRA
            </a-button>
         
          </a-form-item>
        </a-col>
      </a-row>
      
    </a-form>
	</a-card>   

    <Loanding :isVisible="isLoading"/>

  </div>
</template>

<script>
import axios from "axios";
const Swal = require("sweetalert2");
//const je = require("json-encrypt");
import { mapState } from 'vuex'
import Loanding from './../Loanding/Loanding'
import 'moment/locale/es';
//Vue.use(require('vue-moment'));
import moment from 'moment'

export default {
  props: ["id_evento"],

  components: {Loanding},
  data() {
    return {
      isLoading: false,    
      estaodousuario: true,
      validate: false,
         moment,
      modelUsuario: {
        nombre: "",
        apellidos: "",
        correo: "",
        link: "",
        foto:'',
        tema:'',
        id_evento:0,
        cantidad1:0,        
        cantidad2:0,
        cantidad3:0,
        hora_inicio:'',
        hora_fin:''
      },
      errors: {
        nombres: false,
        apellidos: false,        
        correo: false,       
      },
    };
  },
  computed: {
       ...mapState(['url_base'])
  },
  mounted() {
   // this.getTipoUsers();
   this.modelUsuario.id_evento=this.id_evento;
  },
  methods: {
      Validate,   
      onChange(time, timeString) {  
        this.modelUsuario.hora_inicio=timeString;
      },
       onChange1(time, timeString) {  
        this.modelUsuario.hora_fin=timeString;
      },
      AddUser() {     
      let me = this;
      me.isLoading=true;     
      let url = me.url_base+ "Control/Ponente.php";
     // me.modelUsuario.sexo = me.estaodousuario == true ? 1 : 0;     
      const data = me.modelUsuario;
      axios({
        method: "POST",
        url: url,
        data: data,       
      })
        .then(function(response) {
          me.isLoading=false;
                    console.log(response);
          if (response.data.status == "200") {        
             me.MensajeOk("Registrado con Exito !")
          }
           else if(response.data.status="404"){
              Swal.fire({ icon: 'warning', text: response.data.response, timer: 3000,})
          }
        })
        .catch((error) => {
          console.log(error);
           me.isLoading=false;
        });
    },
    ClearText(){
      this.modelUsuario.nombre ="";
      this.modelUsuario.apellidos ="";
      this.modelUsuario.correo ="";    
    },   
     MensajeOk(mensaje){
        Swal.fire({
            title: 'Exito!',
            text: mensaje,        
            icon: 'success',     
            showCancelButton: false,
            confirmButtonColor: '#3085d6',          
            confirmButtonText: 'Ok'
          }).then((result) => {
            if (result.isConfirmed) {
                    this.ClearText();
                   this.validateEquipo = false;
            }
       })
    }

  },
};
function Validate() {
  this.errors.nombre = this.modelUsuario.nombre == "" ? true : false;
  this.errors.apellidos = this.modelUsuario.apellidos == "" ? true : false;
  this.errors.correo = this.modelUsuario.correo == "" ? true : false;

  if (this.errors.nombre) {
    this.validate = true;
    Swal.fire({
      icon: "warning",
      text: "Verifique que campos necesarios esten llenados",
      timer: 2000,
    });
    return false;
  } else {
    this.validate = false;
  }
  if (this.errors.apellidos) {
    this.validate = true;
    Swal.fire({
      icon: "warning",
      text: "Verifique que campos necesarios esten llenados",
      timer: 2000,
    });
    return false;
  } else {
    this.validate = false;
  }
  if (this.errors.correo) {
    this.validate = true;
    Swal.fire({
      icon: "warning",
      text: "Verifique que campos necesarios esten llenados",
      timer: 2000,
    });
    return false;
  } else {
    this.validate = false;
  } 
  
  if (!this.validate) {
    Swal.fire({
      title: "Desea Registrar?",
      text: "",
      icon: "warning",
      showCancelButton: true,
      confirmButtonColor: "#3085d6",
      cancelButtonColor: "#d33",
      confirmButtonText: "Si, Estoy de acuerdo!",
    }).then((result) => {
      if (result.value) {
        this.AddUser();
      }
    });
  }
}
</script>

<style>
.center-cropped {
  object-fit: none; /* Do not scale the image */
  object-position: center; /* Center the image within the element */
  height: 100px;
  width: 100px;
}
</style>
