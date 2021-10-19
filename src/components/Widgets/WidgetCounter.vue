<template>

	<a-card :bordered="false" class="widget-1">
		
		<a-statistic
			:title="title"
			:value="value"		
			:precision="0"
			class="text-success"
			:class="'text-' + status"
		>
		
		</a-statistic>
		
		<div style="display:flex;justify-content: space-between">
			<div class="icon2" ><h5 style="color: #FFFF;">{{meet}}</h5></div>
			<div class="icon3" >	<h5 style="color: #FFFF;">{{face}}</h5></div>
			<div class="icon4" >	<h5 style="color: #FFFF;">{{you}}</h5></div>
		</div>	
	</a-card>

</template>

<script>

	export default ({
		props: {
			title: {
				type: String,
				default: "",
			},
			value: {
				type: String,
				default: "",
			},
			meet: {
				type: Number,
				default: 0,
			},
			face: {
				type: Number,
				default:0,
			},
			you: {
				type: Number,
				default: 0,
			},
			icon: {
				type: String,
				default: "",
			},
			status: {
				type: String,
				default: "success",
			},
		},
		data() {
			return {
			}
		},
	})

</script>