<template>
  <div>


    	<a-card :bordered="false" class="widget-1">

        <div style="display:flex;justify-content: space-between">
         <h3 >{{modelTema.nombre_tema }}  </h3>
          <h3 style="float:right">{{modelTema.dia }} / {{modelTema.fecha}} </h3>
        </div>
    
	    	<div style="display:flex;justify-content: space-between">
       <!-- <h3 >{{modelPonente.nombre }} {{modelPonente.apellidos }} </h3> -->


        <!-- <div>
          <a-input-number size="large" :min="0" :max="100000"  v-model="modelEventoCatec.TotalAsintetes"   @change="(e) => onChange(e, 'meet')" />
      <h5  style=" color: #0dba47;">G. <span style="color:#eab508">Meet </span> </h5>
        </div>
       
       <div>
          <a-input-number size="large" :min="0" :max="100000"  v-model="modelEventoCatec.totalface"  @change="(e) => onChange(e, 'facebook')"  />
  <h5  style=" color: #1871e7;">Facebook  </h5>
        </div>
        <div>
        
        </div> -->
        <h3>{{modelTema.hora_inicio }} - {{modelTema.hora_fin }}</h3>      
      <!-- <h3 style=" color: green;">  {{horaActual2}}</h3> -->
    </div> 
	</a-card><br>  

    <a-row :gutter="24" type="flex">
      
      <a-col :span="18" class="mb-24">
        <!-- <a-button type="primary" @click="AddEstudiante">
          AGREGAR ESTUDIANTE
        </a-button>       -->
      
         
        <!-- Authors Table Card -->
        <TablaAsistencia :data="itemEstudens" :columns="Cabecera"  :cantidad="Total" v-on:Eliminar="Delete" v-on:EnviarAsistencia="EnviarAsistencia"></TablaAsistencia>
      </a-col>
     <a-col :span="6" class="mb-24">
        <!-- <a-button type="primary" @click="AddEstudiante">
          AGREGAR ESTUDIANTE
        </a-button>       -->
      	<a-card :bordered="false" class="widget-1">

     <div style="display:flex;justify-content: space-between">
                 <h5  style=" color: #0dba47;">G. <span style="color:#eab508">Meet </span> </h5>

          <a-input-number size="large" :min="0" :max="100000"  v-model="modelTema.cantidad1"   @change="(e) => onChange(e, 'meet')" />
        </div>
<br> 
  <div style="display:flex;justify-content: space-between">
    <h5  style=" color: #1871e7;">Facebook  </h5>
                <a-input-number size="large" :min="0" :max="100000"  v-model="modelTema.cantidad2"  @change="(e) => onChange(e, 'facebook')"  />
  
        </div> <br> 
          <div style="display:flex;justify-content: space-between">
            <h5  style=" color: #f20000;">YouTube  </h5>
               <a-input-number size="large" :min="0" :max="100000"  v-model="modelTema.cantidad3" @change="(e) => onChange(e, 'youtube')"  />


  
        </div>

	</a-card> 
         
      
      </a-col>

    </a-row>
    <!-- / Authors Table -->
  </div>
</template>

<script>
import axios from "axios";
const Swal = require("sweetalert2");
import { mapState } from 'vuex'
import TablaAsistencia from "@/components/Tablas/TablaAsistencia";
// import Vue from 'vue'
  import 'moment/locale/es';
//Vue.use(require('vue-moment'));
import moment from 'moment'
export default {
  components: {        
    TablaAsistencia
  },
   props: ["datos"],
  data() {
    return {
      itemEstudens: [],
      color:'green',
      Total:0,
      horaActual:moment().format('LTS'),
     
      modelusuario:{id_usuario:0},
      Cabecera: [
          {
          title: "N° Doc.",
          dataIndex: "documento",
         
        },
        {
          title: "NOMBES",
          dataIndex: "nombres",
         
        },
      
        {
          title: "CORREO",
          dataIndex: "correo",
        },
       
        {
          title: "",
          scopedSlots: { customRender: "editBtn" },
          width: 50,
        },
      ],
    modelAsistencia:{ 
      id_evento:0,
      id_estudiante:0,
      id_tema:0,
      fecha:'',
      dia:'',
      hora:'',
      estado:''
    },
      modelTema: {
        id_tema:0,     
        nombre_tema:'',
        id_evento:0,
        cantidad1:0,        
        cantidad2:0,
        cantidad3:0,
        hora_inicio:'',
        hora_fin:'',
        tipo:'',
        fecha:'',
        dia:'',
      },
     modelEventoCatec:{
          id_evento:0,
          id_ponente:0,
          dia:'',
          fecha:'',
          hora:'',
          descripcion:'',
          TotalAsintetes:0,
          totalyou:0,
          totalface:0,
          tipo:'meet'
      },   
        hours: 0,
      minutes: 0,
      seconds: 0
    };
  },
  mounted() {
    this.EstudentList();
    this.Infos();
   
     this.Cantidad();
  },
  computed: {
    ...mapState(['url_base'])  ,
    horatimer(){
      return moment().format('LTS')
    } ,   
    
  }, 
  methods: {
    AddEstudiante() {
      //  this.$router.push({ name: 'UsuariosAdd', params: { id } })
      //this.$router.push({ name: "EstudiantesAdd" });
    }, 
    Infos(){
      if (this.datos) {
        var array =this.datos.split("-");
        var id_evento=array[0];
        var id_tema=array[1];
         this.InfoEventoDia(id_evento);
       //  this.Infoponente(id_tema);
         this.InfoTema(id_tema);
      }
      
    },
    onChange(value,tipo) {           
     let me = this; 
     let url =me.url_base+ "Control/Tema.php"; 
      let object ={id_tema:me.modelTema.id_tema,cantidad1:value,tipo:tipo};     
     let data =object;
      axios({
        method: "PUT",
        url: url,
        data: data    
      })
        .then(function(response) {
        /// alert(response.data.status)
          if (response.data.status == "200") {        
              me.$message.info('Se ha Registrado');
           //  me.InfoEspacio();
          }
           else if(response.data.status="404"){
              Swal.fire({ icon: 'warning', text: response.data.response, timer: 3000,})
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
   
     checkSingleDigit (digit) {
      return ('0' + digit).slice(-2)
    },
     InfoTema(id_tema) {
      let me = this;      
      let url = me.url_base+ "Control/Tema.php?id_tema="+id_tema;      
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.modelTema = response.data;
          //	console.log(response)
        })
        .catch((error) => {
          console.log(error);
        });
    },
    EstudentList() {
      let me = this;
      var items = [];
      let url = me.url_base+ "Control/Estudiante.php";      
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.itemEstudens = response.data;
          //	console.log(response)
        })
        .catch((error) => {
          console.log(error);
        });
    },
    EnviarAsistencia(eve,id_estudiante){
     // alert(eve);
       var mensaje =eve ==true ? "Asistio ! ":"Quitar Asistencia ?"
        Swal.fire({
        title: mensaje,
        text: "se registrara !",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Si, enviar!'
      }).then((result) => {
        if (result.isConfirmed) {
          if (eve) {
             this.Asistio(id_estudiante);
          }else{
            // this.QuitarAsistencia(id_estudiante);
          }
        }
      })  
    },
    Asistio(id){
      let me = this;     
      me.modelAsistencia.id_estudiante= id;
      me.modelAsistencia.estado= 1;
      me.modelAsistencia.dia= me.modelEventoCatec.dia;
      me.modelAsistencia.fecha= me.modelEventoCatec.fecha;
      me.modelAsistencia.hora= moment().format('LTS');
      //	this.horaActual2= moment().format('LTS')
      me.modelAsistencia.id_tema=me.modelTema.id_tema;
      const data = me.modelAsistencia; 
      let url = me.url_base+ "Control/Asistencia.php";  
      axios({
        method: "POST",
        url: url,
        data: data,
      })
        .then(function(response) {
          if (response.data.status == "200") {        
             me.MensajeOk("Se envio con Exito !")
             me.EstudentList();
             me.Cantidad();
          }
           else if(response.data.status="404"){       
             Swal.fire({ icon: 'warning', text: response.data.response, timer: 3000,})
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
     Cantidad(){
      let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=Cantidad";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
           // console.log(response)
            me.Total = response.data.Total;
        
        })
        .catch((error) => {
          console.log(error);
        });
    },
    InfoEventoDia(id_evento){
      let me = this;      
      let url = me.url_base+ "Control/eventoCatec.php?id_evento=" +id_evento;
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
            console.log(response)
            me.modelEventoCatec = response.data.result;
            me.modelAsistencia.id_evento=me.modelEventoCatec.id_evento;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    Eliminar(id){
				let me = this;
				me.modelusuario.id_usuario=id;
				const data = me.modelusuario;    
				///console.log(data); 
				let url =me.url_base+ "Control/usuarioList.php";    
				axios({
					method: "DELETE",
					url: url,
					data: data,
					})
					.then(function(response) {
					//  console.log(response);          
            if (response.data.status == "200") {    
            //	me.modalVisible = false;    
              me.MensajeOk("Eliminado con Exito !")    
              me.usuarioList();        
            //	me.ListVideosEspacio(me.modelVideoOrga.id_espacio,me.modelVideoOrga.id_eventoespacio);							
            //	me.Limpiar();
            }
            else if(response.data.status="404"){
              Swal.fire({ icon: 'warning', text: response.data.msg, timer: 3000,})
            }
					})
					.catch((error) => {
				  	console.log(error);
					});
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
		},
		Limpiar(){
				   	this.modelVideoOrga.nombre_video="";
						this.modelVideoOrga.descrip_video="des";
						this.modelVideoOrga.urlvideo="";
		},
		Delete(id){
				let me =this;
				Swal.fire({
				title: 'Desea Eliminar?',
				text: "Ya no podra revertir!",
				icon: 'warning',
				showCancelButton: true,
				confirmButtonColor: '#3085d6',
				cancelButtonColor: '#d33',
				confirmButtonText: 'Si, Eliminar!'
				}).then((result) => {
				if (result.isConfirmed) {
			    	me.Eliminar(id);
					}
				})
		}
  },
};
</script>

<style lang="scss">


</style>
