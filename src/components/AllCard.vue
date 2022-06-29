<template>
	<div class="card">
		{{ value }}
	</div>
</template>


<script>

export default {
	props:{
		value: {
			type: Number,
			required: true
		}
	}
}
</script>

<style>

</style>
