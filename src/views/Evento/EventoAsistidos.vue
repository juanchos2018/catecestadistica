<template>
  <div>
    <!-- Authors Table -->
    <a-row :gutter="24" type="flex">
    
      <a-col :span="24" class="mb-24">
        
      <h5>Lista q asistieron</h5>
        <!-- Authors Table Card -->
        <TablaAsistio :data="itemEstudens" :columns="Cabecera" v-on:Eliminar="Delete"></TablaAsistio>
      </a-col>
    </a-row>
    <!-- / Authors Table -->
  </div>
</template>

<script>
import axios from "axios";
const Swal = require("sweetalert2");
import { mapState } from 'vuex'
import TablaUser from "@/components/Tablas/TablaUser";
import TablaAsistio from "@/components/Tablas/TablaAsistio";

export default {
  components: {    
    TablaUser,
    TablaAsistio
  },
  props: ["datos"],
  data() {
    return {
      itemEstudens: [],
      modelusuario:{id_usuario:0},
      Cabecera: [
        {
          title: "NOMBES",
          dataIndex: "nombres",
         
        },
        {
          title: "APELLIDOS",
          dataIndex: "apellidos",
         
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

      
    };
  },
  mounted() {
    this.EstudentAdistidos();
  },
  computed: {
    ...mapState(['url_base'])   
  }, 
  methods: {
    AddEstudiante() {
      //  this.$router.push({ name: 'UsuariosAdd', params: { id } })
      this.$router.push({ name: "EstudiantesAdd" });
    },
    EstudentAdistidos() {
      let me = this;
      var array =me.datos.split("-");
      var id_evento=array[0];
      var id_ponente=array[1];
      let url = me.url_base+ "Control/Asistencia.php?id_evento="+id_evento+"&id_ponente="+id_ponente;      
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

<style lang="scss"></style>
