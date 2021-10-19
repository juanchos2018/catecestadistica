<template>


</template>
<script>
	export default ({
		props: {
            id_ponente: {
				type: Number,
				default: 0,
			},
			ponente: {
				type: String,
				default: "",
			},
			tema: {
				type: String,
				default: "",
			},			
			hora_inicio: {
				type: String,
				default: "",
			},
			hora_fin: {
				type: String,
				default: "",
			},
		},
		data() {
			return {
				placements: ['bottomLeft', 'bottomCenter', 'bottomRight', 'topLeft', 'topCenter', 'topRight'],

			}
		},
        methods:{
             Asistencia(id_ponente){
               this.$emit("Asistencia");                
            },
			Asistidos(id_ponente){
				this.$emit("Asistidos");    
			},
			Dialogopoenente(id_ponente){
				this.$emit("Dialogopoenente");   
			}
        }
	})

</script>

<style>
.cardstyle{
  border: 1px solid #1A86D0;
}
</style>