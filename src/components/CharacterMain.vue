<script>
import { DICE } from '@/constants';
import CharacteristicOut from '@/components/Characteristics/CharacteristicOut';

export default {
  name: 'CharacterMain',
	components: {
		CharacteristicOut
	},
	data() {
  	return {
  		mainStat: {
				'str': { base: 20, minus: [], plus: [], name: 'Сила', short_name: 'Сил' },
				'lvk': { base: 16, minus: [], plus: [], name: 'Ловкость', short_name: 'Лвк' },
				'stm': { base: 14, minus: [], plus: [], name: 'Выносливость', short_name: 'Вын'},
				'int': { base: 10, minus: [], plus: [], name: 'Интеллект', short_name: 'Инт' },
				'mind': { base: 11, minus: [], plus: [], name: 'Мудрость', short_name: 'Мдр' },
				'char': { base: 14, minus: [], plus: [], name: 'Харизма', short_name: 'Хар' },
				},
			modStat: {
				'str': { base: 0, minus: [], plus: [] },
				'lvk': { base: 0, minus: [], plus: [] },
				'stm': { base: 0, minus: [], plus: [] },
				'int': { base: 0, minus: [], plus: [] },
				'mind': { base: 0, minus: [], plus: [] },
				'char': { base: 0, minus: [], plus: [] },
			},
			health: {
  			total: 118,
				current: 118,
				buffed: 0,
			},
		}
	},
	computed: {
  	dice() {
  		return DICE;
		},
		mainStatTotal() {
			const { str, lvk, stm, int, mind, char } = this.mainStat;
			// todo переделать в цикл
  		return {
				'str': { value: str.base - this._arrSum(str.minus) + this._arrSum(str.plus), name: str.name, short_name: str.short_name},
				'lvk': { value: lvk.base - this._arrSum(lvk.minus) + this._arrSum(lvk.plus), name: lvk.name, short_name: lvk.short_name},
				'stm': { value: stm.base - this._arrSum(stm.minus) + this._arrSum(stm.plus), name: stm.name, short_name: stm.short_name},
				'int': { value: int.base - this._arrSum(int.minus) + this._arrSum(int.plus), name: int.name, short_name: int.short_name},
				'mind': { value: mind.base - this._arrSum(mind.minus) + this._arrSum(mind.plus), name: mind.name, short_name: mind.short_name},
				'char': { value: char.base - this._arrSum(char.minus) + this._arrSum(char.plus), name: char.name, short_name: char.short_name},
			}
		},
		modStatTotal() {
			const { str, lvk, stm, int, mind, char } = this.mainStat;
			// todo переделать в цикл
			return {
				'str': this._getModStat(str.base) - this._arrSum(str.minus) + this._arrSum(str.plus),
				'lvk': this._getModStat(lvk.base) - this._arrSum(lvk.minus) + this._arrSum(lvk.plus),
				'stm': this._getModStat(stm.base) - this._arrSum(stm.minus) + this._arrSum(stm.plus),
				'int': this._getModStat(int.base) - this._arrSum(int.minus) + this._arrSum(int.plus),
				'mind': this._getModStat(mind.base) - this._arrSum(mind.minus) + this._arrSum(mind.plus),
				'char': this._getModStat(char.base) - this._arrSum(char.minus) + this._arrSum(char.plus),
			}
		},
	},
	created() {
  	console.log(this.rollDice(this.dice.d20, 2));
	},
	methods: {
		_getModStat(stat) {
			return Math.floor((stat-10)/2);
		},

		/**
		 * Получаем случайное число из диапазона,
		 */
		_getRandomInteger(min = 0, max = 1) {
			const lower = Math.ceil(Math.min(min, max));
			const upper = Math.floor(Math.max(min, max));
			return Math.floor(lower + Math.random() * (upper - lower + 1));
		},

		/**
		 * Складываем все элементы переданного массива
		 * @param arr
		 * @returns {number}
		 * @private
		 */
		_arrSum(arr) {
  		let total = 0;
			for (let i = 0; i < arr.length; i++) {
				total += arr[i];
			}
			return total;
		},

		/**
		 * Кидаем дайсы
		 * @param dice
		 * @param count
		 * @returns {{total: number, rolls: []}}
		 */
		rollDice(dice = 20, count = 1 ) {
			let rolls = [];
			let total = 0;
			for (let i = 0; i < count; i++) {
				const rollResult = this._getRandomInteger(1, dice);
				rolls.push(rollResult);
				total += rollResult;
			}
			return {
				rolls: rolls,
				total: total,
			};
		},
	},
}
</script>


<template>
	<div class="character">
		<characteristic-out :stats="mainStatTotal"/>
	</div>
</template>

<style scoped lang="scss">

@media #{"(max-width: 768px)"} {
	.character {
		background-color: red;
	}
}

</style>
