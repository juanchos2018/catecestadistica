<template>
  <div>
      
    <h5> Recargar(F5) </h5>

 <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="12" class="mb-24">
        <a-card :bordered="false" class="dashboard-bar-chart">
           <VueApexCharts type="bar" :options="chartOptions" :series="series"></VueApexCharts>

          <div class="card-title">
            <h6> segun por Dominio de correo</h6>
            <p><span class="text-success">+23</span></p>
          </div>
        </a-card>
      </a-col>
        <a-col :span="24" :lg="12" class="mb-24">
        <a-card :bordered="false" class="dashboard-bar-chart">
           <VueApexCharts type="bar" :options="chartOptions1" :series="series1"></VueApexCharts>

          <div class="card-title">
            <h6>Asistentes segun por modalidad de registro</h6>
            <p><span class="text-success">+23</span></p>
          </div>
        </a-card>
      </a-col>
    </a-row>




  </div>
</template>

<script>

import VueApexCharts from 'vue-apexcharts'

import axios from "axios";
const Swal = require("sweetalert2");
import { mapState } from "vuex";
export default {
  components: {   
  VueApexCharts,
 
  },
  data() {
    return {
  
      itemgrafico7:[],
      itemgrafico8:[],

       series: [
        {
          data: [],
        },
      ],
      chartOptions: {
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




       series1: [
        {
          data: [],
        },
      ],
      chartOptions1: {
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


   }
  },
   computed: {
    ...mapState(["url_base"]),
  },
  mounted() {
      this.Lsitagrafico7();
      this.Lsitagrafico8();
  },
  methods: {
    
    Lsitagrafico7() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico11";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
       
           me.itemgrafico7 = response.data;
           me.itemgrafico7.forEach((item) => {
               me.chartOptions.xaxis.categories.push(item.dominio);
               me.series[0].data.push(item.cantidad);
          });
           //lista1.forEach(item=>{
        })
        .catch((error) => {
          console.log(error);
        });
    },
    
    Lsitagrafico8() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico12";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
      
           me.itemgrafico8 = response.data;
           me.itemgrafico8.forEach((item) => {
               me.chartOptions1.xaxis.categories.push(item.institucion);
               me.series1[0].data.push(item.cantidad);
          });
          
        })
        .catch((error) => {
          console.log(error);
        });
    },
  }
}
</script>

<style>

</style>