<template>
	<div class="alert alert-secondary">
		<h3>{{ x }} + {{ y }} = ?</h3>
		<div class="buttons">
			<button class="btn btn-primary" 
				v-for="number in answers"
				:key="number.id"
				@click="onAnswer(number)"
			>{{ number }}</button>
		</div>
	</div>
</template>

<script>
	export default {
		props: ['settings'],
		data(){
			return {
				x: mtRand(this.settings.from, this.settings.to),
				y: mtRand(this.settings.from, this.settings.to)

			}
		},
		computed: {
			good(){
				return this.x + this.y;
			},
			answers() {
				let res = [this.good];

				while (res.length < this.settings.variants) {
					let num = mtRand(this.good - this.settings.range, this.good + this.settings.range);
					if (res.indexOf(num) === -1) {
						res.push(num);
					}
				}
				res = shuffle(res);
				return res;

			}
		},
		methods: {
			onAnswer(num){
				if (num == this.good) {
					this.$emit('success');
				} else {
					this.$emit('error', `${this.x} + ${this.y} = ${this.good}!`);
				}
			}
		},

	}
	function mtRand(min, max) {
		let diff = max - min;
		return Math.floor(Math.random() * (diff + 1)) + min;
	}
	function shuffle(array) {
		var currentIndex = array.length, temporaryValue, randomIndex;
		// While there remain elements to shuffle...
		while (0 !== currentIndex) {
			// Pick a remaining element...
			randomIndex = Math.floor(Math.random() * currentIndex);
			currentIndex -= 1;
			// And swap it with the current element.
			temporaryValue = array[currentIndex];
			array[currentIndex] = array[randomIndex];
			array[randomIndex] = temporaryValue;
		}

		return array;
	}
</script>

<style scoped lang="scss">
	h3 {
		margin: 0 0 30px;
	}
	.buttons {
		-webkit-display: flex;
		-moz-display: flex;
		-ms-display: flex;
		-o-display: flex;
		display: flex;
		-webkit-justify-content: center;
		-moz-justify-content: center;
		-ms-justify-content: center;
		-o-justify-content: center;
		justify-content: center;
		.btn {
			margin-left: 10px;
			margin-right: 10px;
		}
	}
</style>