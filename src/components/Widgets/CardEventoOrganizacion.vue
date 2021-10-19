<template>
  <!-- Salary Card -->
  <a-card :bordered="true"  class="widget-2 h-full cardstyle">
    <img
      slot="cover"
      alt="example"
      src="images/catec.jpg"
       width="100"
       height="200"
    />
   
    <div style="padding: 10px;">
      		<strong><h5>{{dia}}</h5> </strong>	
          <label for="">{{fecha}}</label>	
      <p>{{ hora }}</p>
    </div>
    <template slot="actions" class="ant-card-actions">
         <a-tooltip placement="top">
        <template slot="title">
          <span>Temas</span>
        </template>
      <a-icon type="picture" @click="Temas(id_evento)" />
      </a-tooltip>

    <!-- <a-tooltip placement="top">
        <template slot="title">
          <span>Ver</span>
        </template>
       <a-icon type="video-camera" @click="Asistidos(id_evento)" />
      </a-tooltip> -->

  <!-- <a-tooltip placement="top">
        <template slot="title">
          <span>Ponentes</span>
        </template>
  <a-icon type="area-chart" @click="Ponentes(id_evento)" />    
  
     </a-tooltip> -->
     
    </template>
  </a-card>
  <!-- / Salary Card -->
</template>

<script>
export default {
  name: "card-eventos-organizacion",
  props: {
    id_evento: {
      type: Number,
      default: 0,
    },
    fecha:{
       type:String,
       default:'' 
    },
    dia: {
      type: String,
      default: "",
    },
    hora: {
      type: String,
      default: "",
    },
    descripcion: {
      type: String,
      default: "",
    },
    
  },
  data() {
    return {     

    };
  },
 methods:{
   Temas(id_evento){
     this.$router.push({
              name: "TemasEvento",
              params: { id_evento: id_evento},
        });
   },
     Ponencias(id_evento){
       this.$router.push({
              name: "EventoPonente",
              params: { id_evento: id_evento},
        });
     },
     Asistencia(id_evento){
     //  alert(id_evento)
        // var datos=id_eventoespacio+"-"+id_espacio
        //  console.log(id_orgaevento+"-"+id_espacio);        
            this.$router.push({
              name: "EventoAsistencia",
              params: { id_evento: id_evento},
        });
    },
    Asistidos(id_evento){
      this.$router.push({
              name: "EventoAsistidos",
              params: { id_evento: id_evento},
        });
    },
    Ponentes(id_evento){
         this.$router.push({
              name: "PonenteList",
              params: { id_evento: id_evento},
        });
    },
    videosOrganizacion(id_orgaevento){
        //  var datos=id_espacio+"-"+id_eventoespacio
         // this.$router.push({ name: "VideoOrganEvento" ,  params: { id_orgaevento: id_orgaevento},});
    }
 }
};
</script>
<style>
.cardstyle{
  border: 1px solid #1A86D0;
}
</style>