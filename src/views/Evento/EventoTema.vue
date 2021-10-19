<template>
  <div>
    <a-button type="primary" @click="AddTema">
      AGREGAR TEMA
    </a-button>
    <br />
    <br />
    <a-row type="flex" :gutter="24">
      <a-col
        :span="12"
        :xl="8"
        class="mb-24"
        v-for="item in itemtemas"
        :key="item.id_tema"
      >
        <a-card :bordered="false" class="widget-1 cardstyle">
          <h5>{{ item.nombre_tema }}</h5>
          <h5>
            {{ item.hora_inicio }}- {{ item.hora_fin }} / {{ item.fecha }}
          </h5>

          <div>
            <a-dropdown style="float:right">
              <a-button> Obciones</a-button>
              <a-menu slot="overlay">
                <a-menu-item>
                  <a
                    target="_blank"
                    rel="noopener noreferrer"
                   @click="Dialogopoenente(item.id_tema)"
                    >Add Ponente</a
                  >

                </a-menu-item>
                <a-menu-item>
                  <a
                    target="_blank"
                    rel="noopener noreferrer"
                   @click="Asistencia(item.id_tema)"
                    >Asisntcia</a
                  >
                </a-menu-item>
                <a-menu-item>
                  <a
                    target="_blank"
                    rel="noopener noreferrer"
                    
                    >Lista</a
                  >
                </a-menu-item>
              </a-menu>
            </a-dropdown>
          </div>
          <br /><br />

          <div style="display:flex">
            <div v-for="item1 in item.Lista" :key="item1.id_ponente">
              <a-tooltip placement="top">
                <template slot="title">
                  <span>{{ item1.nombre }}</span>
                </template>
                <a-avatar style="margin-left:5px" icon="user" />
              </a-tooltip>
            </div>
          </div>        
        </a-card>       
      </a-col>
    </a-row>

    <!-- Diaglo agregar nuevo tema -->
    <a-modal :visible="dialogtema" :footer="null" @cancel="handleCancelModal">
      <a-form @submit.prevent="Validate">
        <a-form-item label="nombre_tema">
          <a-input placeholder="nombre_tema" v-model="modelTema.nombre_tema">
          </a-input>
        </a-form-item>
        <a-form-item label="Fecha">
          <a-date-picker style="width: 50%" v-model="modelTema.fecha" />
          <a-input
            disabled
            style="width: 40%;margin-left:20px"
            placeholder="Dia"
            v-model="diaevento"
          >
          </a-input>
        </a-form-item>
        <a-form-item label="Hora Inicio   /   Hora Termino ">
          <a-time-picker
            :default-value="moment('12:08', 'HH:mm')"
            format="HH:mm"
            @change="onChange"
          />

          <a-time-picker
            style="margin-left:40px"
            :default-value="moment('12:08', 'HH:mm')"
            format="HH:mm"
            @change="onChange1"
          />
        </a-form-item>        
        <a-form-item>
          <a-button type="primary" html-type="submit">
            REGISTRAR
          </a-button>
        </a-form-item>
      </a-form>
    </a-modal>

    <!-- dialog ponentne -->
    <a-modal
      :visible="dialogponente"
      :footer="null"
      @cancel="handleCancelModal"
    >
      <a-form @submit.prevent="ValidatePonente">
        <a-form-item label="nombre">
          <a-input placeholder="nombre poennente" v-model="modelPonente.nombre">
          </a-input>
        </a-form-item>
        <a-form-item label="apellidos">
          <a-input placeholder="apellidos" v-model="modelPonente.apellidos">
          </a-input>
        </a-form-item>
        <a-form-item label="correo">
          <a-input placeholder="correo" v-model="modelPonente.correo">
          </a-input>
        </a-form-item>
        <a-form-item label="link">
          <a-input placeholder="link" v-model="modelPonente.link"> </a-input>
        </a-form-item>

        <a-form-item>
          <a-button type="primary" html-type="submit">
            REGISTRAR
          </a-button>
        </a-form-item>
      </a-form>
    </a-modal>

    <Loanding :isVisible="isLoading" />
  </div>
</template>

<script>
import axios from "axios";
const Swal = require("sweetalert2");
import WidgetSalary from "@/components/Widgets/WidgetSalary";
import CardEventoOrganizacion from "@/components/Widgets/CardEventoOrganizacion";

import { mapState } from "vuex";
import Loanding from "./../Loanding/Loanding";
import "moment/locale/es";
import moment from "moment";
import WidgetPonencias from "@/components/Widgets/WidgetPonencias";

export default {
  name: "mis-organizaciones",
  props: ["id_evento"],

  components: {
    WidgetSalary,
    CardEventoOrganizacion,
    Loanding,
    WidgetPonencias,
  },
  data() {
    return {
      fechaa: "",
      fe2: "",
      f3: "",
      placements: [
        "bottomLeft",
        "bottomCenter",
        "bottomRight",
        "topLeft",
        "topCenter",
        "topRight",
      ],

      horass: moment().format("LT"),
      hora: moment().format("LT"),
      moment,
      momentssss: "",
      broadcast_date: moment(new Date())
        .local()
        .format("DD-MM-YYYY"),

      isLoading: false,
      estaodousuario: true,
      validate: false,
      previewImage: "",
      fileList: [],
      itemFotosOrga: [],
      previewVisible: false,
      itemEspacios: [],
      itemEventoDia: [],
      itemponecias: [],
      modelEventoCatec2: {
        id_evento: 0,
        dia: "",
        fecha: moment(new Date())
          .local()
          .format("YYYY-MM-DD"),
        hora: moment().format("LT"),
        descripcion: "",
      },

      modelEventoCatec: {
        id_evento: 0,
        id_ponente: 0,
        dia: "",
        fecha: "",
        hora: "",
        descripcion: "",
        TotalAsintetes: 0,
        totalyou: 0,
        totalface: 0,
        tipo: "meet",
      },
      errors: {
        dia: false,
        fecha: false,
        hora: false,
      },
      dialogevento: false,
      validate: false,
      validatepone: false,
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 12 },
      },
      dialogtema: false,
      dialogponente: false,
      itemtemas: [],
      modelTema: {
        id_tema: 0,
        nombre_tema: "",
        dia: "",
        fecha: "",
        hora_inicio: "",
        hora_fin: "",
        id_evento: 0,
        cantidad1: 0,
        cantidad2: 0,
        cantidad3: 0,
      },
      modelPonente: {
        nombre: "",
        apellidos: "",
        correo: "",
        link: "",
        foto: "",
        tema: "",
        id_evento: 0,
        id_tema: 0,
        cantidad1: 0,
        cantidad2: 0,
        cantidad3: 0,
      },
    };
  },
  created() {},
  mounted() {
    this.TemasLista(); //bien
    //    this.InfoEventoDia();
    this.modelTema.id_evento = this.id_evento;
    this.modelPonente.id_evento = this.id_evento;
  },
  computed: {
    ...mapState(["url_base"]),
    diaevento: function() {
      // `this` points to the vm instance
      return moment(this.modelTema.fecha).format("dddd");
      //  return this.modelEventoCatec.fecha .split('').reverse().join('')
    },
    plusOneHour() {
      return moment(this.hora)
        .add(1, "hours")
        .format("HH:mm:ss");
    },
  },
  methods: {
    Validate,
    ValidatePonente,
    AddTema() {
      this.dialogtema = true;
    },
    DialogoEVento() {
      this.dialogevento = true;
    },
    Dialogopoenente(id_tema) {
      this.dialogponente = true;
      this.modelPonente.id_tema = id_tema;
    },
    onChange(time, timeString) {
      this.modelTema.hora_inicio = timeString;
    },
    onChange1(time, timeString) {
      this.modelTema.hora_fin = timeString;
    },

    AddTEma() {
      let me = this;
      me.isLoading = true;
      let url = me.url_base + "Control/Tema.php";
      me.modelTema.dia = me.diaevento;
      me.modelTema.fecha = moment(me.modelTema.fecha).format("DD-MM-YYYY");
      const data = me.modelTema;
      axios({
        method: "POST",
        url: url,
        data: data,
      })
        .then(function(response) {
          me.isLoading = false;
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
          me.isLoading = false;
          console.log(error);
        });
    },
    TemasLista() {
      let me = this;
      let url = me.url_base + "Control/Tema.php?id_evento=" + me.id_evento;
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.itemtemas = response.data;
          //console.log(response)
        })
        .catch((error) => {
          console.log(error);
        });
    },

    AddPonente() {
      let me = this;
      me.isLoading = true;
      let url = me.url_base + "Control/Ponente.php";

      const data = me.modelPonente;
      axios({
        method: "POST",
        url: url,
        data: data,
      })
        .then(function(response) {
          me.isLoading = false;
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
          me.isLoading = false;
          console.log(error);
        });
    },

    InfoEventoDia() {
      let me = this;
      let url =
        me.url_base + "Control/eventoCatec.php?id_evento=" + me.id_evento;
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          console.log(response);
          me.modelEventoCatec = response.data.result;
          //  me.modelAsistencia.id_evento=me.modelEventoCatec.id_evento;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    Asistencia(id_tema) {
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
      let datos = this.id_evento + "-" + id_tema;
      this.$router.push({
        name: "EventoAsistencia",
        params: { datos: datos },
      });
    },
    Asistidos(id_ponente) {
      var datos = this.id_evento + "-" + id_ponente;
      this.$router.push({
        name: "EventoAsistidos",
        params: { datos: datos },
      });
    },
    handleCancelModal() {
      this.dialogtema = false;
      this.dialogponente = false;
    },

    PonenteList() {
      let me = this;
      let url = me.url_base + "Control/Ponente.php?id_evento=" + me.id_evento;
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.itemponecias = response.data;
          console.log(response);
        })
        .catch((error) => {
          console.log(error);
        });
    },

    ListEventoCatec() {
      let me = this;
      let url = me.url_base + "Control/eventoCatec.php";
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
      me.isLoading = true;
      let url = me.url_base + "Control/eventoCatec.php";
      me.modelEventoCatec.dia = me.diaevento;
      me.modelEventoCatec.fecha = moment(me.modelEventoCatec.fecha).format(
        "DD-MM-YYYY"
      );
      // moment(response.data.fechacrea).format('YYYY-MM-DD')
      const data = me.modelEventoCatec;

      axios({
        method: "POST",
        url: url,
        data: data,
      })
        .then(function(response) {
          me.isLoading = false;
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
          me.isLoading = false;
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

function ValidatePonente() {
  if (!this.validatepone) {
    Swal.fire({
      title: "Desea Registrar Poennte?",
      text: "",
      icon: "warning",
      showCancelButton: true,
      confirmButtonColor: "#3085d6",
      cancelButtonColor: "#d33",
      confirmButtonText: "Si, Estoy de acuerdo!",
    }).then((result) => {
      if (result.value) {
        this.dialogponente = false;
        this.AddPonente();
      }
    });
  }
}

function Validate() {
  this.errors.dia = this.modelTema.dia == "" ? true : false;
  this.errors.fecha = this.modelTema.fecha == "" ? true : false;

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
        this.dialogtema = false;
        this.AddTEma();
      }
    });
  }
}
</script>
