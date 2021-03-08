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
				'agl': { base: 16, minus: [], plus: [], name: 'Ловкость', short_name: 'Лвк' },
				'stm': { base: 14, minus: [], plus: [], name: 'Выносливость', short_name: 'Вын'},
				'int': { base: 10, minus: [], plus: [], name: 'Интеллект', short_name: 'Инт' },
				'wisdom': { base: 11, minus: [], plus: [], name: 'Мудрость', short_name: 'Мдр' },
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
			BMA: [11, 6, 1],
			currentAttack: 1,
			health: {
  			total: 118,
				current: 118,
				buffed: 0,
			},
			currentWeapon: {
  			title: '',
				name: '',
				damage: {},
				multiplier: 1,
			},
			armorItem: [
				{
					title: 'pojas-telesnogo-sovershenstva',
					name: 'Пояс небесного совершенства',
					bonus: 2,
					plus: [
						{'str': 1},
						{'agl': 1},
						{'stm': 1}
					],
					minus: [],
					active: true,
				},
			],
			weaponItem: {
				kosa: {
					title: 'isk-kosa-vampirizma',
					name: 'Искусная коса вампиризма',
					bonus: 2,
					damage: { count: 4, dice: 4},
					description: 'осстанавливает половину нанесенного урона',
					active: true,
					multiplier: 1.5,
				}
			},
		}
	},
	computed: {
  	dice() {
  		return DICE;
		},
		mainStatTotal() {
			const { str, agl, stm, int, wisdom, char } = this.mainStat;
			// todo переделать в цикл
  		return {
				'str': { value: str.base - this._arrSum(str.minus) + this._arrSum(str.plus), name: str.name, short_name: str.short_name},
				'agl': { value: agl.base - this._arrSum(agl.minus) + this._arrSum(agl.plus), name: agl.name, short_name: agl.short_name},
				'stm': { value: stm.base - this._arrSum(stm.minus) + this._arrSum(stm.plus), name: stm.name, short_name: stm.short_name},
				'int': { value: int.base - this._arrSum(int.minus) + this._arrSum(int.plus), name: int.name, short_name: int.short_name},
				'wisdom': { value: wisdom.base - this._arrSum(wisdom.minus) + this._arrSum(wisdom.plus), name: wisdom.name, short_name: wisdom.short_name},
				'char': { value: char.base - this._arrSum(char.minus) + this._arrSum(char.plus), name: char.name, short_name: char.short_name},
			}
		},
		modStatTotal() {
			const { str, agl, stm, int, wisdom, char } = this.mainStat;
			// todo переделать в цикл
			return {
				'str': this._getModStat(str.base) - this._arrSum(str.minus) + this._arrSum(str.plus),
				'agl': this._getModStat(agl.base) - this._arrSum(agl.minus) + this._arrSum(agl.plus),
				'stm': this._getModStat(stm.base) - this._arrSum(stm.minus) + this._arrSum(stm.plus),
				'int': this._getModStat(int.base) - this._arrSum(int.minus) + this._arrSum(int.plus),
				'wisdom': this._getModStat(wisdom.base) - this._arrSum(wisdom.minus) + this._arrSum(wisdom.plus),
				'char': this._getModStat(char.base) - this._arrSum(char.minus) + this._arrSum(char.plus),
			}
		},
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

		checkHit() {
			const roll = this.rollDice(20);
			const mod = this.BMA[this.currentAttack] + this.modStatTotal.str;
			const result = { value: roll.total + mod, rolls: roll.rolls, type: (roll.total === (20 || 1) ? 'crit' : 'normal')};
			console.log('проверка попадания: ', result);
			return result;
		},
		checkDamage() {
			this.currentWeapon = this.weaponItem.kosa;
			const damage = this.rollDice(this.currentWeapon.damage.dice,  this.currentWeapon.damage.dice);
			const result = Math.round(this.modStatTotal.str * this.currentWeapon.multiplier) + damage.total;
			console.log(damage);
			console.log(result);
			return result;
		},
	},
}
</script>


<template>
	<div class="character">
		<characteristic-out :stats="mainStatTotal" :mods="modStatTotal"/>
		<el-button @click="checkHit">Проверка на попадание</el-button>
		<el-button @click="checkDamage">Нанесение урона</el-button>
	</div>
</template>

<style scoped lang="scss">

@media #{"(max-width: 768px)"} {
	.character {
		background-color: red;
	}
}

</style>
