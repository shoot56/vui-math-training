<template>
	<div class="alert alert-secondary">
		<h3>Are You ready?</h3>
		<button class="btn btn-success" @click="$emit('onStart')">
			Start
		</button>
	</div>
</template>

<script>
	export default {
		
	}
</script>

<style scoped lang="scss">
	.alert {
		margin: 0;
		text-align: center;
	}
	.btn {
		margin: 20px 0;
	}
</style>