<template>
  <div>
    <h5>Lista de Dias Evento Catec</h5>
    <a-button type="primary" @click="DialogoEVento">
      AGREGAR DIA EVENTO
    </a-button>
    <br /><br />
    
    <a-row type="flex" :gutter="24">
      <a-col
        :span="12"
        :xl="6"
        class="mb-24"
        v-for="item in itemEventoDia"
        :key="item.id_evento"
      >
        <card-evento-organizacion
          :id_evento="item.id_evento"
          :fecha="item.fecha"
          :dia="item.dia"
          :hora="item.hora"         
        ></card-evento-organizacion>
      </a-col>
    </a-row>

    <a-modal :visible="dialogevento" :footer="null" @cancel="handleCancelModal">
      <a-form @submit.prevent="Validate" >
        <a-form-item label="Fecha">
           <a-date-picker
              style="width: 100%"
              v-model="modelEventoCatec.fecha"
            />            
            <!-- <p>{{fechaa | moment("dddd, MMMM Do YYYY")}}</p>
            <span>{{ fechaa | moment("dddd, MMMM Do YYYY") }}</span> <br>
            <span>{{ fechaa | moment("dddd") }}</span> <br>
               <span>{{ fechaa | moment("DD, MMMM Do YYYY")}}</span> <br>
                <span>{{ fechaa | moment("DD-MM-YYYY")}}</span> -->
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
//  import moment from 'moment';
 // import Vue from 'vue'
  import 'moment/locale/es';
//Vue.use(require('vue-moment'));
import moment from 'moment'

//var moment = require("moment");

export default {
  name: "mis-organizaciones",
  components: {
    WidgetSalary,
    CardEventoOrganizacion,
    Loanding
  },
  data() {
    return {
        fechaa:'',
        fe2:'',
        f3:'',
        horass:moment().format('LT'),hora:moment().format('LT'),
        moment,
        momentssss:'',
        broadcast_date: moment(new Date()).local().format("YYYY-MM-DD"),
      
      isLoading: false,
      estaodousuario: true,
      validate: false,
      previewImage: "",
      fileList: [],
      itemFotosOrga: [],
      previewVisible: false,
      itemEspacios: [],
      itemEventoDia: [],

      modelEventoCatec:{
          id_evento:0,
          dia:'',
          fecha:moment(new Date()).local().format("YYYY-MM-DD"),
          hora:moment().format('LT'),
          descripcion:'',
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
     this.ListEventoCatec();
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
    handleCancelModal() {
      this.dialogevento = false;
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
