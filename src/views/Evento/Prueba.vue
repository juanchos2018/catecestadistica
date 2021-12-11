<template>
  <div>
    <h5> Recargar(F5) </h5>

 <a-row :gutter="24" type="flex" align="stretch">
      <a-col :span="24" :lg="12" class="mb-24">
        <a-card :bordered="false" class="dashboard-bar-chart">
           <VueApexCharts type="bar" :options="chartOptions2" :series="series2"></VueApexCharts>

          <div class="card-title">
            <h4>Asistentes segun por modalidad de registro</h4>
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
  
       chartOptions2: {
            chart: {
              type: 'bar',
              height: 350
            },
            plotOptions: {
              bar: {
                borderRadius: 4,
                horizontal: true,
              }
            },
            legend: {
            fontSize: "42px",
              fontFamily: "Helvetica, Arial, sans-serif",
                fontWeight: "bold"
              },
           dataLabels: {
              enabled: true,
               style: {
                fontSize: "20px",
                fontFamily: "Helvetica, Arial, sans-serif",
                fontWeight: "bold"
              }
            },
            colors: [
                      function({ value, seriesIndex, w }) {
                        return '#'+(0x1000000+(Math.random())*0xffffff).toString(16).substr(1,6)
                       
                      }
           ],
         
            xaxis: {
              categories: [],
               labels: {
                  show: true,
                            maxHeight: 120,

                  style: {  
                             

                      fontSize: '18px',
                         fontWeight: 600,
                                 cssClass: 'apexcharts-xaxis-label'

                  }
            }
            }
          },
      series2: [{
            data: []
          }],

   }
  },
   computed: {
    ...mapState(["url_base"]),
  },
  mounted() {
      this.Lsitagrafico7();
  },
  methods: {
    
    Lsitagrafico7() {
      let me = this;
      let url = me.url_base + "Control/Consulta.php?tipo=grafico8";
      axios({
        method: "GET",
        url: url,
      })
        .then(function(response) {
          console.log(response)
           me.itemgrafico7 = response.data;
           me.itemgrafico7.forEach(item=>{
               me.chartOptions2.xaxis.categories.push(item.dominio)
               me.series2[0].data.push(item.cantidad)
           })
           //lista1.forEach(item=>{
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