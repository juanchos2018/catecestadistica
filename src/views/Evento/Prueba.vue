<template>
  <div>
    <h5> prueba we</h5>


     <VueApexCharts type="bar" :options="chartOptions" :series="series"></VueApexCharts>


  </div>
</template>

<script>
import VueApexCharts from 'vue-apexcharts'

export default {
  components: {   
  VueApexCharts,
 
  },
  data() {
    return {
  
          series: [{
            data: [400, 430, 448, 470, 540, 580, 690, 1100, 1200, 1380]
          }],
          
          chartOptions: {
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
            dataLabels: {
              enabled: false
            },
            colors: [
                      function({ value, seriesIndex, w }) {
                        return '#'+(0x1000000+(Math.random())*0xffffff).toString(16).substr(1,6)
                       
                      }
                 ],
         
            xaxis: {
              categories: ['South Korea', 'Canada', 'United Kingdom', 'Netherlands', 'Italy', 'France', 'Japan',
                'United States', 'China', 'Germany'
              ],
            }
          },


   }
  }
}
</script>

<style>

</style>