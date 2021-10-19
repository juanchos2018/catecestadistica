<template>
  <div>
    <!-- Counter Widgets -->
     <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="12" class="mb-24"  v-for="item in evntosDias"
        :key="item.evntosDias">
         <a-card :bordered="false" class="dashboard-bar-chart">    

       <VueApexCharts type="area" height="350" :options="chartOptions" :series="item.series"></VueApexCharts>
        	<div class="card-title">
			<h6>Grafico de Total Visitantes del dia {{item.dia}}</h6>    
			<p><span class="text-success">+23</span></p>
		</div>
		
       </a-card>
      </a-col>    
    </a-row>
<hr>
    <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="12" class="mb-24"  v-for="item in TotalAsistentes"
        :key="item.id_evento">
         <a-card :bordered="false" class="dashboard-bar-chart">  
        <VueApexCharts type="bar" height="350" :options="item.chartOptions" :series="item.series"></VueApexCharts>
      	<div class="card-title">
			<h6>Grafico de Total Asitencia del dia {{item.dia}}</h6>    
			<p><span class="text-success">+23</span></p>
		</div>
		
       </a-card>
      </a-col>    
    </a-row>

<br>
  <!-- grfico 3 -->
 <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="8" class="mb-24"  v-for="item in itemgrafico3"
        :key="item.id_ponente">
         <a-card :bordered="false" class="dashboard-bar-chart">  
        <VueApexCharts type="pie" :options="item.chartOptions" :series="item.series"></VueApexCharts>
      	<div class="card-title">
			<h6>Grafico de Total  </h6>    
			<p><span class="text-success">+23</span></p>
		</div>
		
       </a-card>
      </a-col>    
    </a-row>
<!-- grfico 4 -->
 <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="8" class="mb-24"  v-for="item in itemgrafico4"
        :key="item.id_ponente">
         <a-card :bordered="false" class="dashboard-bar-chart">  
        <VueApexCharts type="donut" :options="item.chartOptions" :series="item.series"></VueApexCharts>
      	<div class="card-title">
			<h6>Grafico de Sexo  </h6>    
			<p><span class="text-success">+23</span></p>
		</div>
		
       </a-card>
      </a-col>    
    </a-row>


    <a-row :gutter="24">
      <a-col
        :span="24"
        :lg="12"
        :xl="6"
        class="mb-24"
        v-for="item in itemvisitantes"
        :key="item.id_evento"
      >     
      <h5>ddd</h5> 
        <!-- <WidgetCounter
          :title="item.dia"
          :value="item.fecha"        
         
          :status="status"
          :meet="item.TotalAsintetes"        
          :face="item.totalface"
          :you="item.totalyou"
        ></WidgetCounter>      -->

      </a-col>
    </a-row>  

    <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="12" class="mb-24">
        <!-- <CardBarChart></CardBarChart> -->
        <a-card :bordered="false" class="dashboard-bar-chart">  
       <MapChart
  :countryData="{'US': 4, 'CA': 7, 'GB': 8, 'IE': 14, 'ES': 21}"
  highColor="#ff0000"
  lowColor="#aaaaaa"
  countryStrokeColor="#909090"
  defaultCountryFillColor="#dadada"
  LangUser="es"
  />       </a-card>

      </a-col>
      <a-col :span="24" :lg="12" class="mb-24" >
       <!-- <VueApexCharts type="bar" height="350" :options="chartOptions" :series="series"></VueApexCharts> -->
       <CardBarChart2></CardBarChart2>
      
      </a-col>
    </a-row>

   


    <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="24" class="mb-24">
            <!-- <TablaAsistentesTotal :data="itemTotalAsistentes" :columns="Cabecera1" ></TablaAsistentesTotal> -->

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
import VueApexCharts from 'vue-apexcharts'
//import TablaAsistentesTotal from "../components/Tablas/TablaAsistentesTotal";
//import MapChart from 'vue-map-chart'
import MapChart from 'vue-chart-map'

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
  MapChart
//  TablaAsistentesTotal
  },
  data() {
    return {
	status:'success',
 
    itemvisitantes: [{id_espacio:1,nombre_espacio:'espacio1',visitantes:5},{id_espacio:2,nombre_espacio:'espacio 2',visitantes:10}],
	  itemmensajes:[],
    evntosDias:[],
    itemgrafico3:[],
    TotalAsistentes:[],
    itemgrafico4:[],

   series: [14, 23, 21, 17, 15, 10, 12],
   chartOptions2: {
            chart: {
              type: 'polarArea',
            },
            labels: ['Rose A', 'Rose B', 'Rose C', 'Rose D', 'Rose E','Rose E','Rose E'],
            stroke: {
              colors: ['#fff']
            },
            fill: {
              opacity: 0.8
            },
            responsive: [{
              breakpoint: 480,
              options: {
                chart: {
                  width: 200
                },
                legend: {
                  position: 'bottom'
                }
              }
            }]
          },




     chartOptions1: {
            chart: {
              height: 350,
              type: 'bar',
            },
            plotOptions: {
              bar: {
                borderRadius: 10,
                dataLabels: {
                  position: 'top', // top, center, bottom
                },
              }
            },
            dataLabels: {
              enabled: true,
              formatter: function (val) {
                return val + "%";
              },
              offsetY: -20,
              style: {
                fontSize: '12px',
                colors: ["#304758"]
              }
            },            
            xaxis: {
              categories: ["criptomondeas", "videjuegos", "hackint"],              
              axisBorder: {
                show: false
              },
              axisTicks: {
                show: false
              },
              crosshairs: {
                fill: {
                  type: 'gradient',
                  gradient: {
                    colorFrom: '#D8E3F0',
                    colorTo: '#BED1E6',
                    stops: [0, 100],
                    opacityFrom: 0.4,
                    opacityTo: 0.5,
                  }
                }
              },
              tooltip: {
                enabled: true,
              }
            },

            yaxis: {
              axisBorder: {
                show: false
              },
              axisTicks: {
                show: false,
              },
              labels: {
                show: false,
                formatter: function (val) {
                  return val + "%";
                }
              }
            
            },
           
          },

     chartOptions: {
            chart: {
              height: 390,
              type: 'area'
            },
            dataLabels: {
              enabled: false
            },
            stroke: {
              curve: 'smooth'
            },
            xaxis: {
              type: 'string',
              categories: ["meet", "facebook", "youtube"],
              axisBorder: {
                show: false
              },
              axisTicks: {
                show: false
              },
            },
            yaxis: {
              axisBorder: {
                show: false
              },
              axisTicks: {
                show: false,
              },
              labels: {
                show: false,
                formatter: function (val) {
                  return val + "%";
                }
              }
            
            },
            tooltip: {
              x: {
                format: 'dd/MM/yy HH:mm'
              },
            },
      },
    
      itemTotalAsistentes: [],
      modelusuario:{id_usuario:0},
      
      Cabecera1: [
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
          title: "ASISTENCIAS",
          dataIndex: "asistencias",
        },
      ],
   
    };
  },
   computed: {
    ...mapState(['url_base'])   
  },
  mounted() {  
   //  this.LsitaTotalAsistntes();    
     // this.MostrarTotalVisitantes();

     this.LsitaEventoDia(); // ok
     this.LsitaTodalPorDiaPonente();
     this.Lsitagrafico3();
     this.Lsitagrafico4();
   },
  methods: {  
      LsitaEventoDia() {
	    let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=grafico1";      
      axios({
        method: "GET",
        url: url,
       })
        .then(function(response) {       
            me.evntosDias=response.data;         
        })
        .catch((error) => {
          console.log(error);
        });
	  },
    LsitaTodalPorDiaPonente() {
	    let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=grafico2";      
      axios({
        method: "GET",
        url: url,
       })
        .then(function(response) {       
            me.TotalAsistentes=response.data;         
        })
        .catch((error) => {
          console.log(error);
        });
	  },

     Lsitagrafico3() {
	    let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=grafico3";
      
      axios({
        method: "GET",
        url: url,
       })
        .then(function(response) {       
            me.itemgrafico3=response.data;
        })
        .catch((error) => {
          console.log(error);
        });
	  },

   Lsitagrafico4() {
	    let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=grafico4";      
      axios({
        method: "GET",
        url: url,
       })
        .then(function(response) {       
            me.itemgrafico4=response.data;
        })
        .catch((error) => {
          console.log(error);
        });
	  },

//itemgrafico4
	  LsitaTotalAsistntes() {
	    let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=TotalesAsisCerti";
      
      axios({
        method: "GET",
        url: url,
       })
        .then(function(response) {       
            me.itemTotalAsistentes=response.data;
        })
        .catch((error) => {
          console.log(error);
        });
	  },    

    MostrarTotalVisitantes(){
      let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=Total";
     
      axios({
        method: "GET",
        url: url,
       })
        .then(function(response) {       
           me.itemvisitantes=response.data;      
         
        })
        .catch((error) => {
          console.log(error);
        });
    }
   
  },
};
</script>

<style lang="scss"></style>
