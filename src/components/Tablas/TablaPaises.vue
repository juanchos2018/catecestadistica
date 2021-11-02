<template>
	<a-card :bordered="false" class="header-solid h-full" :bodyStyle="{padding: 0,}">
		<template #title>
			<a-row type="flex" align="middle">
				<a-col :span="24" :md="12">
					<h5 class="font-semibold m-0">Segun Pais</h5>
				</a-col>				
			</a-row>
		</template>
		<a-table :columns="columns" :data-source="data" :pagination="true" :row-key="data => data.index">
						
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
				authorsHeaderBtns: 'all',
			}
		},
		 computed: {
  		  
		},
		methods:{           
		}
	})

</script>