<template>
	<a-card :bordered="false" class="header-solid h-full" :bodyStyle="{padding: 0,}">
		<template #title>
			<a-row type="flex" align="middle">
				<a-col :span="24" :md="12">
					<h5 class="font-semibold m-0">Lista Estudiantes</h5>
				</a-col>				
			</a-row>
		</template>
		<a-table :columns="columns" :data-source="data" :pagination="true" :row-key="data => data.id_estudiante">
			
			<template slot="editBtn" slot-scope="data">
				<a-button type="primary" :data-id="data.id_estudiante" @click="Ver(data.id_estudiante)">
					Ver
				</a-button>					
			</template>
		</a-table>
	</a-card>
</template>

<script>

import { mapState } from 'vuex'
	export default ({
		props: {
			data: {
				type: Array,
				default: () => [],
			},
			columns: {
				type: Array,
				default: () => [],
			},
		},
		data() {
			return {
				// Active button for the "Authors" table's card header radio button group.
				authorsHeaderBtns: 'all',
			}
		},
		 computed: {
  		  ...mapState(['url_base'])		
		},
		methods:{
            Ver(id){			
			   //this.$router.push({ name: "UsuarioEdit" ,params:{ id_usuario:id}});
			},
			Eliminar(id){
				//this.$emit('Eliminar',id);
			}
			
		}
	})

</script>