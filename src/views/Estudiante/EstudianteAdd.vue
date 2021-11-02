<template>
  <div>
	<a-card :bordered="false" class="header-solid h-full" :bodyStyle="{padding: 4,}">
    <a-form @submit.prevent="Validate">
      <a-row :gutter="[24, 24]">
          <a-col :span="24" :md="6">
          <a-form-item label="Correo">
            <a-input placeholder="correo" v-model="modelUsuario.correo">
            </a-input>
          </a-form-item>
        </a-col>

          <a-col :span="24" :md="6">
          <a-form-item label="documento">
            <a-input placeholder="documento" v-model="modelUsuario.documento">
            </a-input>
          </a-form-item>
        </a-col>

        <a-col :span="24" :md="8">
          <a-form-item label="Nombres Apellidos">
            <a-input placeholder="Nombres" v-model="modelUsuario.nombres">
            </a-input>
          </a-form-item>
        </a-col>

         <a-col :span="24" :md="4">
          <a-form-item label="edad">
            <a-input placeholder="edad" v-model="modelUsuario.edad">
            </a-input>
          </a-form-item>
        </a-col>
             


       <a-col :span="12" :md="2">
          <a-form-item label="sexo">
                <a-switch checked-children="M" un-checked-children="F" default-checked v-model="estaodousuario" />
         
          </a-form-item>
        </a-col> 
         <a-col :span="24" :md="4">
          <a-form-item label="pais">
            <a-input placeholder="pais" v-model="modelUsuario.pais">
            </a-input>
          </a-form-item>
        </a-col>

          <a-col :span="24" :md="4">
          <a-form-item label="telefno">
            <a-input placeholder="telefno" v-model="modelUsuario.telefono">
            </a-input>
          </a-form-item>
        </a-col>


        <a-col :span="24" :md="4">
          <a-form-item label="Nivel">
          <a-select style="width: 100%"  v-model="modelUsuario.nivel">
          <a-select-option v-for="item in nivel" :key="item">
            {{ item }}
          </a-select-option>
        </a-select>
          </a-form-item>
        </a-col>

      <a-col :span="24" :md="4">
          <a-form-item label="Ciclo">
            <a-select style="width: 100%"  v-model="modelUsuario.ciclo">
          <a-select-option v-for="item in ciclos" :key="item">
            {{ item }}
          </a-select-option>
        </a-select>
          </a-form-item>
        </a-col>

  <a-col :span="24" :md="4">
          <a-form-item label="grado">
            <a-select style="width: 100%"  v-model="modelUsuario.grado">
          <a-select-option v-for="item in grado" :key="item">
            {{ item }}
          </a-select-option>
        </a-select>
          </a-form-item>
        </a-col>
      </a-row>
       
      <a-row :gutter="[24, 24]">

    
        <a-col :span="24" :md="6">
          <a-form-item label="institucion">
            <a-input placeholder="institucion" v-model="modelUsuario.institucion">
            </a-input>
          </a-form-item>
        </a-col>
        
       

        <a-col :span="24" :md="3">
          <a-form-item label="Registrar">
            <a-button type="primary" html-type="submit" >
              REGISTRAR
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


export default {
  components: {Loanding},
  data() {
    return {
      isLoading: false,    
      estaodousuario: true,
      validate: false,
      modelUsuario: {
        nombres: "",
        apellidos: " ",
        ciclo: "",
        grado:'',
        documento:'',
        fecha_registro:'',
        telefeno:'',
        institucion:'Universidad Privada de Tacna',        
        correo: "",
        estado:0,
        edad:'',
        sexo:0,
        pais:'Perú',
        nivel:''
      },
      errors: {
        nombres: false,
        apellidos: false,        
        correo: false,       
      },
     nivel : ['Secundaria','Técnico','Universitario','Profesional','Maestrías o más','otros'],
     ciclos : ['1er Ciclo','2do Ciclo','3er Ciclo','4to Ciclo','5to Ciclo','6to Ciclo','7mo Ciclo','8vo Ciclo','9no Ciclo','10mo Ciclo','T'],
     grado : ['1ro de Secundaria','2do de Secundaria','3ro de Secundaria','4to de Secundaria','5to de Secundaria'],


      
    };
  },
  computed: {
       ...mapState(['url_base'])
  },
  mounted() {
   // this.getTipoUsers();
  },
  methods: {
      Validate,   
      AddUser() {     
      let me = this;
      me.isLoading=true;     
      let url = me.url_base+ "Control/Estudiante.php";
      me.modelUsuario.sexo = me.estaodousuario == true ? 1 : 0;     
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
      this.modelUsuario.nombres ="";
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
                  //  this.ClearText();
                   this.validateEquipo = false;
            }
       })
    }

  },
};
function Validate() {
  this.errors.nombres = this.modelUsuario.nombres == "" ? true : false;
  this.errors.apellidos = this.modelUsuario.apellidos == "" ? true : false;
  this.errors.correo = this.modelUsuario.correo == "" ? true : false;

  if (this.errors.nombres) {
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
