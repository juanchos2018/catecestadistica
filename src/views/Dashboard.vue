<template>
  <div>
  
    <h5>Visitantes de las plataformas</h5>
    <a-row :gutter="24" type="flex" align="stretch">
      <a-col
        :span="24"
        :lg="12"
        class="mb-24"
        v-for="item in evntosDias"
        :key="item.evntosDias"
      >
        <a-card :bordered="false" class="dashboard-bar-chart">
          <VueApexCharts
            type="area"
            height="350"
            :options="chartOptions"
            :series="item.series"
          ></VueApexCharts>
          <div class="card-title">
            <h6>Grafico de Total Visitantes del dia {{ item.dia }}</h6>
            <p><span class="text-success">+23</span></p>
          </div>
        </a-card>
      </a-col>
    </a-row>
    <hr />

    <h5>Estudio de los Asistentes por Ponencia</h5>

    <a-row :gutter="24" type="flex" align="stretch">
      <a-col
        :span="24"
        :lg="12"
        class="mb-24"
        v-for="item in TotalAsistentes"
        :key="item.id_evento"
      >
        <a-card :bordered="false" class="dashboard-bar-chart">
          <VueApexCharts
            type="bar"
            height="350"
            :options="item.chartOptions"
            :series="item.series"
          ></VueApexCharts>
          <div class="card-title">
            <h6>Grafico de Total Asitencia del dia {{ item.dia }}</h6>
            <p><span class="text-success">+23</span></p>
          </div>
        </a-card>
      </a-col>
    </a-row>

    <br />

    <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="24" class="mb-24">
      
        <a-card :bordered="false" class="dashboard-bar-chart">
          <VueApexCharts
            type="line"
            height="350"
            :options="itemgrafico8.chartOptions"
            :series="itemgrafico8.series"
          ></VueApexCharts>
          <div class="card-title">
            <h6>DIAGRAMA DE EVOLUCION DEL EVENTO CATEC 2021-II</h6>
            <p>
              <span class="text-success">ddd</span>
            </p>
          </div>
        </a-card>
      </a-col>
    </a-row>



   
    <h5>Estudio de Nivel Academico por Ponencia</h5>
    <a-row :gutter="24" type="flex" align="stretch">
      <a-col
        :span="24"
        :lg="10"
        class="mb-24"
        v-for="item in itemgrafico3"
        :key="item.id_ponente"
      >
        <a-card :bordered="false" class="dashboard-bar-chart">
          <VueApexCharts
            type="pie"
            :options="item.chartOptions"
            :series="item.series"
          ></VueApexCharts>
          <div class="card-title">
            <h6>Grafico de Por Nivel Academico</h6>
            <p>
              <span class="text-success">{{ item.tema }}</span>
            </p>
          </div>
        </a-card>
      </a-col>
    </a-row>
    

 
    <h5>Estudio de Sexo por Ponencia</h5>
    <a-row :gutter="24" type="flex" align="stretch">
      <a-col
        :span="24"
        :lg="10"
        class="mb-24"
        v-for="item in itemgrafico4"
        :key="item.id_ponente"
      >
        <a-card :bordered="false" class="dashboard-bar-chart">
          <VueApexCharts
            type="donut"
            :options="item.chartOptions"
            :series="item.series"
          ></VueApexCharts>
          <div class="card-title">
            <h4>Grafico de Sexo</h4>
            
            <h5 class="text-success">{{ item.tema }}</h5>
            
          </div>
        </a-card>
      </a-col>
    </a-row>

    <h5>Estudio de Edad por Ponencia</h5>
    <a-row :gutter="24" type="flex" align="stretch">
      <a-col
        :span="24"
        :lg="24"
        class="mb-24"
        v-for="item in itemgrafico6"
        :key="item.id_tema"
      >
        <a-card :bordered="false" class="dashboard-bar-chart">
          <VueApexCharts
            type="bar"
            height="350"
            :options="item.chartOptions"
            :series="item.series"
          ></VueApexCharts>
          <div class="card-title">
            <h6>Grafico de Edad</h6>
            <p>
              <span class="text-success">{{ item.tema }}</span>
            </p>
          </div>
        </a-card>
      </a-col>
    </a-row>


 <h5>Estudio de Asistentes de la Universidad</h5>
    <a-row :gutter="24" type="flex" align="stretch">
      <a-col
        :span="24"
        :lg="14"
        class="mb-24"
        v-for="item in itemgrafico9"
        :key="item.id_tema"
      >
        <a-card :bordered="false" class="dashboard-bar-chart">
          <VueApexCharts
            type="bar"
            height="350"
            :options="item.chartOptions"
            :series="item.series"
          ></VueApexCharts>
          <div class="card-title">
            <h6>Grafico de Asistentes de UPT</h6>
            <p>
              <span class="text-success">{{ item.tema }}</span>
            </p>
          </div>
        </a-card>
      </a-col>
    </a-row>


    <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="12" class="mb-24">
        <a-card :bordered="false" class="dashboard-bar-chart">
          <VueApexCharts
            type="bar"
            :options="chartOptions2"
            :series="series2"
          ></VueApexCharts>

          <div class="card-title">
            <h6>Asistentes segun por modalidad de registro</h6>

          </div>
        </a-card>
      </a-col>
    </a-row>
    
   
    <h5>Estudio de Lugar de Procedencia</h5>
    <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="16" class="mb-24">
        <a-card
          :bordered="false"
          class="dashboard-bar-chart"
          style="background-color:#010101"
        >
          <MapChart
            :countryData="{
              PE: 230,
              CL: 1,
              BO: 12,
              CO: 5,
              EC: 8,
              ES: 2,
              HO: 1,
              AR: 1,
            }"
            highColor="#ff0000"
            lowColor="#aaaaaa"
            countryStrokeColor="#909090"
            defaultCountryFillColor="#dadada"
            LangUser="es"
          />
        </a-card>
      </a-col>
      <a-col :span="24" :lg="8" class="mb-24">
        <a-card :bordered="false" class="dashboard-bar-chart">
          <TablaPaises :data="itemgrafico5" :columns="Cabecera"></TablaPaises>
        </a-card>
      </a-col>
    </a-row>

    <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="24" class="mb-24">
        <a-card :bordered="false" class="dashboard-bar-chart">
          <TablaTotalAsistentes
            :data="itemTotalAsistentes"
            :columns="Cabecera5"
          ></TablaTotalAsistentes>
        </a-card>
      </a-col>
    </a-row>

  </div>
</template>

<script>
import axios from "axios";
const Swal = require("sweetalert2");
import { mapState } from "vuex";
// Bar chart for "Active Users" card.
//import CardBarChart from "../components/Cards/CardBarChart";
import CardBarChart2 from "../components/Cards/CardBarChart2";

// Line chart for "Sales Overview" card.
import CardLineChart from "../components/Cards/CardLineChart";
// Counter Widgets
//import WidgetCounter from "../components/Widgets/WidgetCounter";
// "Projects" table component.
import CardProjectTable from "../components/Cards/CardProjectTable";
// Order History card component.
import CardOrderHistory from "../components/Cards/CardOrderHistory";
// Information card 1.
import CardInfo from "../components/Cards/CardInfo";
// Information card 2.
import CardInfo2 from "../components/Cards/CardInfo2";
import TablaMensaje from "../components/Tablas/TablaMensaje";
import TablaPaises from "../components/Tablas/TablaPaises";
import TablaTotalAsistentes from "../components/Tablas/TablaTotalAsistentes";
import VueApexCharts from "vue-apexcharts";
import MapChart from "vue-chart-map";

export default {
  components: {
    // CardBarChart,
    CardLineChart,
    ///  WidgetCounter,
    CardProjectTable,
    CardOrderHistory,
    CardInfo,
    CardInfo2,
    TablaMensaje,
    CardBarChart2,
    VueApexCharts,
    TablaPaises,
    MapChart,
    TablaTotalAsistentes,
    //  TablaAsistentesTotal
  },
  data() {
    return {
      status: "success",     
      itemmensajes: [],
      evntosDias: [],
      itemgrafico3: [],
      TotalAsistentes: [],
      itemgrafico4: [],
      itemgrafico5: [],
      itemgrafico6: [],
      itemgrafico7: [],
      itemgrafico8: {},
      itemgrafico9: [],
      itemTotalAsistentes: [],
      itemTotalAsistentes: [],
      series: [14, 23, 21, 17, 15, 10, 12],

      chartOptions: {
        chart: {
          height: 390,
          type: "area",
        },
        dataLabels: {
          enabled: false,
        },
        stroke: {
          curve: "smooth",
        },
        xaxis: {
          type: "string",
          categories: ["meet", "facebook", "youtube"],
          axisBorder: {
            show: false,
          },
          axisTicks: {
            show: false,
          },
        },
        yaxis: {
          axisBorder: {
            show: false,
          },
          axisTicks: {
            show: false,
          },
          labels: {
            show: false,
            formatter: function(val) {
              return val + "%";
            },
          },
        },
        tooltip: {
          x: {
            format: "dd/MM/yy HH:mm",
          },
        },
      },
      modelusuario: { id_usuario: 0 },

      Cabecera: [
        {
          title: "PAIS",
          dataIndex: "pais",
        },
        {
          title: "TOTAL",
          dataIndex: "cantidad",
        },
      ],
      Cabecera5: [
        {
          title: "NOMBES",
          dataIndex: "nombres",
        },
        {
          title: "ASISTENCIAS",
          dataIndex: "cantidad",
        },
      ],
      series2: [
        {
          data: [],
        },
      ],
      chartOptions2: {
        chart: {
          type: "bar",
          height: 350,
        },
        plotOptions: {
          bar: {
            borderRadius: 4,
            horizontal: true,
          },
        },
        dataLabels: {
          enabled: false,
        },
        colors: [
          function({ value, seriesIndex, w }) {
            return (
              "#" +
              (0x1000000 + Math.random() * 0xffffff).toString(16).substr(1, 6)
            );
          },
        ],

        xaxis: {
          categories: [],
        },
      },

      
      series3: [{
              name: "Desktops",
              data: []
          }],
        chartOptions3: {
            chart: {
              height: 350,
              type: 'line',
              zoom: {
                enabled: false
              }
            },
            dataLabels: {
              enabled: false
            },
            stroke: {
              curve: 'straight'
            },
            title: {
              text: 'Product Trends by Month',
              align: 'left'
            },
            grid: {
              row: {
                colors: ['#f3f3f3', 'transparent'], // takes an array which will be repeated on columns
                opacity: 0.5
              },
            },
            xaxis: {
              categories: [],
            }
          },


    };
  },
  computed: {
    ...mapState(["url_base"]),
  },
  mounted() {
    //  this.LsitaTotalAsistntes();
  
    this.LsitaEventoDia(); // ok
    this.LsitaTodalPorDiaPonente();
    this.Lsitagrafico3();
    this.Lsitagrafico4();
    this.Lsitagrafico5();
    this.Lsitagrafico6();
    this.Lsitagrafico7();
    this.Lsitagrafico8();
    this.LsitagraficoMayorAsistentes();
    this.Lsitagrafico9();
  },
  methods: {
    LsitagraficoMayorAsistentes() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico7";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.itemTotalAsistentes = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    LsitaEventoDia() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico1";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.evntosDias = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    LsitaTodalPorDiaPonente() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico2";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.TotalAsistentes = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    Lsitagrafico3() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico3";

      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          // console.log(response)
          me.itemgrafico3 = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    Lsitagrafico4() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico4";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.itemgrafico4 = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    Lsitagrafico5() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico5";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.itemgrafico5 = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    Lsitagrafico6() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico6";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.itemgrafico6 = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    Lsitagrafico7() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico8";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          console.log(response);
          me.itemgrafico7 = response.data;
          me.itemgrafico7.forEach((item) => {
            me.chartOptions2.xaxis.categories.push(item.dominio);
            me.series2[0].data.push(item.cantidad);
          });
          //lista1.forEach(item=>{
        })
        .catch((error) => {
          console.log(error);
        });
    },
    Lsitagrafico8() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico9";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          // console.log(response)
          me.itemgrafico8 = response.data;
          me.series3[0].data=response.data.series[0].data;
          me.chartOptions3.xaxis.categories=response.data.chartOptions.xaxis.categories;
          //lista1.forEach(item=>{
        })
        .catch((error) => {
          console.log(error);
        });
    },
 Lsitagrafico9() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico10";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.itemgrafico9 = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    //itemgrafico4-*--------------------------
    LsitaTotalAsistntes() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=TotalesAsisCerti";

      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          me.itemTotalAsistentes = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    
  },
};
</script>

<style lang="scss"></style>
