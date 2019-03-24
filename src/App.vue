<template>
	<div class="training">
		<div class="container">
			<h1>Math training </h1>
			<h4>level: {{ levels[level].name }} <br>question {{ current }} from {{ levels[level].questMax }}</h4>
			<div class="progress">
				<div class="progress-bar" :style="progressStyles"></div>
			</div>
			<div class="box">
				<transition name="flip" mode="out-in">
					<app-start-screen 
						v-if="state == 'start'"
						@onStart="onStart"
					>
					</app-start-screen>
					<app-question 
						v-else-if="state == 'question'"
						@success="onQuestSuccess"
						@error="onQuestError"
						:settings="levels[level]"
					></app-question>
					<app-message 
						v-else-if="state == 'message'"
						:type="message.type"
						:text="message.text"
						@onNext="onNext"
					></app-message>
					<app-result-screen 
						v-else-if="state == 'results'"
						:stats="stats"
						:level="level"
						@repeat="onStart"
						@nextLevel="onnextLevel"
						@toStart="toStartLevel"
					></app-result-screen>
					<div v-else>Unknow state</div>
				</transition>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'app',
	data () {
		return {
			state: 'start' ,
			stats: {
				success: 0,
				error: 0
			},
			message: {
				type: '',
				text: ''
			},
			level: 0,
			current: 1,
			levels: [
				{
					questMax: 4,
					name: 'easy',
					variants: 2,
					from: 10,
					to: 20,
					range: 5
				},
				{
					questMax: 6,
					name: 'normal',
					variants: 4,
					from: 100,
					to: 200,
					range: 20
				},
				{
					questMax: 8,
					name: 'hard',
					variants: 6,
					from: 500,
					to: 990,
					range: 43
				}
			]
		}
	},
	computed: {
		questDone(){
			return this.stats.success + this.stats.error;
		},
		progressStyles(){
			return{
				width: (this.questDone/this.levels[this.level].questMax*100) + '%'
			};
		}
	},
	methods: {
		onStart(){
			this.state="question";
			this.stats.success = 0;
			this.stats.error = 0;
			this.current = 1;
		},
		onQuestSuccess(){
			this.state="message";
			this.message.text = 'Good Job';
			this.message.type = 'success';
			this.stats.success++;

			// alert(this.levels.length - 1);
		},
		onQuestError(msg){
			this.state="message";
			this.message.text = msg;
			this.message.type = 'danger';
			this.stats.error++;
		},
		onNext(){
			if (this.questDone < this.levels[this.level].questMax) {
				this.state="question";
				this.current++;
			} else {
				this.state="results";
			}
		},
		onnextLevel(){
			this.level++;
			this.onStart();
		},
		toStartLevel(){
			this.level = 0;
			this.onStart();
		}
	}
}
</script>

<style lang="scss">
.training {
	padding: 40px 0;
}
.container {
	max-width: 700px;
}
.progress {
	margin: 20px 0;
}
.buttons {
	text-align: center;
}
.btn {
	margin: 20px 0;
}
.flip {
	&-enter{
		&-active{
			animation: flipInX .3s linear;
		}
	}
	&-leave{
		&-active{
			animation: flipOutX .3s linear;
		}
	}
}
.progress-bar {
	-webkit-transition: width 0.3s;
	-moz-transition: width 0.3s;
	-o-transition: width 0.3s;
	transition: width 0.3s;
}
@keyframes flipInX {
	from{
		transform: rotateX(90deg);
	}
	to {
		transform: rotateX(0deg);
	}
}
@keyframes flipOutX {
	from{
		transform: rotateX(0deg);
	}
	to {
		transform: rotateX(90deg);
	}
}

</style>
