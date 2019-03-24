<template>
	<div class="alert" :class="'alert-' + type">
		<h3>{{ text }}</h3>
		<div class="buttons">
			<button class="btn btn-primary" @click="$emit('onNext')">Continue</button>
		</div>
	</div>
</template>

<script>
	export default {
		props: ['type', 'text']
	}
</script>

<style lang="scss">
	
</style>