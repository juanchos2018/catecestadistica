<template>
  <div>
    <h5>  Ponencias del dia {{modelEventoCatec.dia}} / {{modelEventoCatec.fecha}} </h5>
  <p>{{broadcast_date}}</p>
    <br /><br />
    
    <a-row type="flex" :gutter="24">
      <a-col
        :span="12"
        :xl="6"
        class="mb-24"
        v-for="item in itemponecias"
        :key="item.id_ponente"
      >
         <WidgetPonencias
          :id_ponente="item.id_ponente"
          :ponente="item.nombre + item.apellidos"
          :tema="item.tema"
          :hora_inicio="item.hora_inicio"
          :hora_fin="item.hora_fin"  
          v-on:Asistencia="Asistencia(item.id_ponente)"
          v-on:Asistidos="Asistidos(item.id_ponente)"
        ></WidgetPonencias>   
      </a-col>
    </a-row>

    <a-modal :visible="dialogevento" :footer="null" @cancel="handleCancelModal">
      <a-form @submit.prevent="Validate" >
        <a-form-item label="Fecha">
           <a-date-picker
              style="width: 100%"
              v-model="modelEventoCatec.fecha"
            />            
           
        </a-form-item>
          <a-form-item label="Dia">
            <a-input    disabled  style="width: 60%;margin-right:20px" placeholder="Dia"  v-model="diaevento">
            </a-input>
              <a-time-picker :default-value="moment('12:08', 'HH:mm')" format="HH:mm"  @change="onChange"  />
          </a-form-item>
     
         <a-form-item label="descripcion">
            <a-input placeholder="descripcion" v-model="modelEventoCatec.descripcion">
            </a-input>
          </a-form-item>        
        <a-form-item>
          <a-button type="primary" html-type="submit">
            REGISTRAR
          </a-button>
        </a-form-item>
      </a-form>
    </a-modal>
        <Loanding :isVisible="isLoading"/>

  </div>
</template>

<script>
import axios from "axios";
const Swal = require("sweetalert2");
import WidgetSalary from "@/components/Widgets/WidgetSalary";
import CardEventoOrganizacion from "@/components/Widgets/CardEventoOrganizacion";

import { mapState } from 'vuex'
import Loanding from './../Loanding/Loanding'
import 'moment/locale/es';
import moment from 'moment'
import WidgetPonencias from "@/components/Widgets/WidgetPonencias";


export default {
  name: "mis-organizaciones",
  props: ["id_evento"],

  components: {
    WidgetSalary,
    CardEventoOrganizacion,
    Loanding,
    WidgetPonencias
  },
  data() {
    return {
        fechaa:'',
        fe2:'',
        f3:'',
        horass:moment().format('LT'),hora:moment().format('LT'),
        moment,
        momentssss:'',
        broadcast_date: moment(new Date()).local().format("DD-MM-YYYY"),
      
      isLoading: false,
      estaodousuario: true,
      validate: false,
      previewImage: "",
      fileList: [],
      itemFotosOrga: [],
      previewVisible: false,
      itemEspacios: [],
      itemEventoDia: [],
      itemponecias:[],
      modelEventoCatec2:{
          id_evento:0,
          dia:'',
          fecha:moment(new Date()).local().format("YYYY-MM-DD"),
          hora:moment().format('LT'),
          descripcion:'',
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
      errors: {
        dia: false,
        fecha: false,
        hora: false        
      },
      dialogevento: false,
      validate: false,
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 12 },
      },
    };
  },
 created() {		 	
 },
  mounted() {   
     this.PonenteList(); //bien
     this.InfoEventoDia();
  },
  computed: {
      ...mapState(['url_base']),
      diaevento: function () {
      // `this` points to the vm instance
       return  moment(this.modelEventoCatec.fecha).format('dddd')
    //  return this.modelEventoCatec.fecha .split('').reverse().join('')
    },
    plusOneHour() {
        return moment(this.hora).add(1, 'hours').format('HH:mm:ss');
    },
  },
  methods: {
    Validate,   
    DialogoEVento() {
      this.dialogevento = true;
    },
    onChange(time, timeString) {
  
      this.modelEventoCatec.hora=timeString;
    },
     InfoEventoDia(){
      let me = this;      
      let url = me.url_base+ "Control/eventoCatec.php?id_evento="+me.id_evento;
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
            console.log(response)
            me.modelEventoCatec = response.data.result;
          //  me.modelAsistencia.id_evento=me.modelEventoCatec.id_evento;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    Asistencia(id_ponente){
    
    // if (this.broadcast_date !=this.modelEventoCatec.fecha) {
    //      Swal.fire({
    //   icon: "warning",
    //   text: "Esta Ponencia no ha comenzado o ya termino",
    //   timer: 2000,
    // });
    // }else{
    //     let datos=this.id_evento+"-"+id_ponente;
    //      this.$router.push({
    //           name: "EventoAsistencia",
    //           params: { datos: datos},
    //      });
    // }
         let datos=this.id_evento+"-"+id_ponente;
         this.$router.push({
              name: "EventoAsistencia",
              params: { datos: datos},
         });
    },
    Asistidos(id_ponente){
        var datos =this.id_evento+"-"+id_ponente
        this.$router.push({
              name: "EventoAsistidos",
              params: { datos: datos},
        });
    },
    handleCancelModal() {
      this.dialogevento = false;
    },
     PonenteList() {
      let me = this;     
      let url = me.url_base+ "Control/Ponente.php?id_evento="+me.id_evento;      
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
            me.itemponecias = response.data;
           	console.log(response)
        })
        .catch((error) => {
          console.log(error);
        });
    },

    ListEventoCatec() {      
      let me = this; 
      let url = me.url_base+ "Control/eventoCatec.php";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.itemEventoDia = response.data;
      //    console.log(response);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    AddEVentoEspacio() {
      let me = this;    
      me.isLoading=true;
      let url = me.url_base+"Control/eventoCatec.php";    
      me.modelEventoCatec.dia=me.diaevento;
      me.modelEventoCatec.fecha =moment( me.modelEventoCatec.fecha).format("DD-MM-YYYY");
     // moment(response.data.fechacrea).format('YYYY-MM-DD')
      const data = me.modelEventoCatec;  

      axios({
        method: "POST",
        url: url,
        data: data,
      })
        .then(function(response) {
           me.isLoading=false;
          if (response.data.status == "200") {
             me.MensajeOk("Registrado con Exito !");
           ///  me.ListEventoCatec();
          } else if ((response.data.status = "404")) {
            Swal.fire({
              icon: "warning",
              text: response.data.response,
              timer: 3000,
            });
          }
        })
        .catch((error) => {
           me.isLoading=false;
          console.log(error);
        });
    },

    MensajeOk(mensaje) {
      Swal.fire({
        title: "Exito!",
        text: mensaje,
        icon: "success",
        showCancelButton: false,
        confirmButtonColor: "#3085d6",
        confirmButtonText: "Ok",
      }).then((result) => {
        if (result.isConfirmed) {
          ///                  this.ClearText();
          this.validateEquipo = false;
        }
      });
    },
  },
};

function Validate() {
  this.errors.dia =
    this.modelEventoCatec.dia == "" ? true : false;
  this.errors.fecha =
    this.modelEventoCatec.fecha == "" ? true : false;
    

  ///let me = this;
  if (!this.validate) {
    Swal.fire({
      title: "Desea Registrar ?",
      text: "",
      icon: "warning",
      showCancelButton: true,
      confirmButtonColor: "#3085d6",
      cancelButtonColor: "#d33",
      confirmButtonText: "Si, Estoy de acuerdo!",
    }).then((result) => {
      if (result.value) {
        this.dialogevento = false;
        this.AddEVentoEspacio();
      }
    });
  }
}
</script>
