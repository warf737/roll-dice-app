<script>
import { DICE } from '@/constants';

export default {
  name: 'HelloWorld',
	data() {
  	return {
  		mainStats: [],
			health: [],

		}
	},
	computed: {
  	dice() {
  		return DICE;
		}
	},
	created() {
  	console.log(this.rollDice(this.dice.d20, 2));
	},
	methods: {
  	rollDice(dice = 20, count = 1 ) {
  		let rolls = [];
  		let total = 0;
  		for (let i = 0; i < count; i++) {
  			const rollResult = this.getRandomInteger(1, dice);
  			rolls.push(rollResult);
  			total += rollResult;
			}
			return {
				rolls: rolls,
				total: total,
			};
		},
		getRandomInteger(min = 0, max = 1) {
			const lower = Math.ceil(Math.min(min, max));
			const upper = Math.floor(Math.max(min, max));
			return Math.floor(lower + Math.random() * (upper - lower + 1));
		},
	},
}
</script>


<template>
	<div class="hello">
		hello
	</div>
</template>

<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
