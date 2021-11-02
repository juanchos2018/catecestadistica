<template>
  <!-- Active Users Card -->
 <a-card :bordered="false" class="dashboard-bar-chart">    
      <VueApexCharts width="500" type="bar" :options="options" :series="series"></VueApexCharts>
		<div class="card-title">
			<h6>Total Asistentes 2</h6>    
			<p><span class="text-success">+23</span></p>
		</div>
		
		<a-row class="card-footer" type="flex" justify="center" align="top">
			<a-col :span="6">
				<h4>3,6K</h4>
				<span>Users</span>
			</a-col>
			<a-col :span="6">
				<h4>2m</h4>
				<span>Clicks</span>
			</a-col>
			<a-col :span="6">
				<h4>$772</h4>
				<span>Sales</span>
			</a-col>
			<a-col :span="6">
				<h4>82</h4>
				<span>Items</span>
			</a-col>
		</a-row>
   
  </a-card>
  <!-- Active Users Card -->
</template>

<script>
import Vue from "vue";
import axios from "axios";
import { mapState } from "vuex";
import VueApexCharts from 'vue-apexcharts'
const Swal = require("sweetalert2");
Vue.prototype.$eventHub = new Vue();
// Bar chart for "Active Users" card.
import ChartBar from "../Charts/ChartBar";

export default {
  components: {
    ChartBar,VueApexCharts
  },
  data() {
    return {
		 videoId: "lG0Ys-2d4MA",	
     asistentes:[],
      options: {
        chart: {
          id: 'vuechart-example'
        },
        xaxis: {
          categories: []
        }
      },
      series: [{
        name: 'Total',
        data: []
      }],
     barchar:{},
     lista:[],
      // Data for bar chart.
      barChartData: {
        labels: [],
        datasets: [
          {
            label: "Sales",
            backgroundColor: "#fff",
            borderWidth: 0,
            borderSkipped: false,
            borderRadius: 6,
            data: [],
            maxBarThickness: 20,
          },
        ],
      },
    };
  },
  computed: {
    ...mapState(["url_base"]),
	
  },
  created() {
    //this.ListTotalAsistentes();   
  
     console.log("hola")
    //  this.barChartData.datasets[0].data.push(850)
    //    this.barChartData.datasets[0].data.push(600)
  },
   mounted() {
      this.Grafico0();
      //this.barChartData.labels[0].push('1')
      //  this.ListTotalAsistentes2();
 //  this.ListTotalAsistentes();   

  },
   methods: {
     Grafico0(){
       let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=Total";
      var lista1=[];      
      axios({
        method: "GET",
        url: url,
       })
        .then(function(response) {       
        lista1=response.data;
         lista1.forEach(item=>{
                        
                   me.options.xaxis.categories.push(item.dia)
                   me.series[0].data.push(item.TotalAsintetes)
                      
              })
          //  for (let index = 0; index < response.data.labels.length; index++) {           
          //      me.options.xaxis.categories.push(response.data.labels[index]);
          //  }          
          //   me.series[0].data=response.data.datasets[0].data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
      Grafico1(){
       let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=TotalesAsis";
      var lista=[];      
      axios({
        method: "GET",
        url: url,
       })
        .then(function(response) {          
           for (let index = 0; index < response.data.labels.length; index++) {           
               me.options.xaxis.categories.push(response.data.labels[index]);
           }          
            me.series[0].data=response.data.datasets[0].data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
     ListTotalAsistentes2(){
      let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=Totales";
      var lista=[];      
      axios({
        method: "GET",
        url: url,
       })
        .then(function(response) {
           // console.log(response)        
            me.barchar= response.data;   
        })
        .catch((error) => {
          console.log(error);
        });
   },
   ListTotalAsistentes(){
      let me = this;      
      let url = me.url_base+ "Control/Consulta.php?tipo=Total";
      var lista=[];      
      axios({
        method: "GET",
        url: url,
       })
        .then(function(response) {
           // console.log(response)
          //  lista = response.data;
            me.asistentes= response.data;
           
            //  for(let i=0;i< me.asistentes.length ;i++){
            //     alert("recibe" +i);
            //       me.barChartData.labels[0].push(me.asistentes[i].dia)
            //       me.barChartData.datasets[0].data.push(me.asistentes[i].TotalAsintetes)
                                 
            //  }
            var sizr =me.asistentes.length;
          //  alert(sizr);
            response.data.forEach(item=>{
                        alert(item.dia);
                  me.barChartData.labels[0].push(item.dia)
                  me.barChartData.datasets[0].data.push(item.TotalAsintetes)
                      
              })
           
        
        })
        .catch((error) => {
          console.log(error);
        });
   },
  
  },
};
</script>
<style lang="scss" scoped>
@import './blog-style.scss';
</style>