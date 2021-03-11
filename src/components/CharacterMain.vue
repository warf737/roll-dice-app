<script>
import { DICE } from '@/constants';
import CharacteristicOut from '@/components/Characteristics/CharacteristicOut';
import RollResult from '@/components/RollResult/RollResult';
import Combat from '@/components/Combat/Combat';

export default {
  name: 'CharacterMain',
	components: {
		CharacteristicOut,
		RollResult,
		Combat
	},
	data() {
  	return {
			currentLvl: 11,
  		mainStat: {
				'str': { base: 20, minus: [], plus: [], name: 'Сила', short_name: 'Сил' },
				'agl': { base: 16, minus: [], plus: [], name: 'Ловкость', short_name: 'Лвк' },
				'stm': { base: 14, minus: [], plus: [], name: 'Выносливость', short_name: 'Вын'},
				'int': { base: 10, minus: [], plus: [], name: 'Интеллект', short_name: 'Инт' },
				'wisdom': { base: 11, minus: [], plus: [], name: 'Мудрость', short_name: 'Мдр' },
				'char': { base: 14, minus: [], plus: [], name: 'Харизма', short_name: 'Хар' },
				}, // основные статы
			BMA: [11, 6, 1], // Базовый можификатор атаки
			currentAttack: 0, // нужен для определения текущеего БМА
			skillPoints: {
  			multiplier: 4,
				stat: 'int',
  			description: '4 * модификатор Интеллекта за уровень'
			},
			classSkills: [
				{
					title: 'Акробатика',
					mod: 'agl'
				},
				{
					title: 'Верховая езда',
					mod: 'agl'
				},
				{
					title: 'Внимание',
					mod: 'wisdom'
				},
				{
					title: 'Выживание',
					mod: 'wisdom'
				},
				{
					title: 'Дрессировка',
					mod: 'char'
				},
				{
					title: 'Запугивание',
					mod: 'char'
				},
				{
					title: 'Знание природы',
					mod: 'int'
				},
				{
					title: 'Лазание',
					mod: 'str'
				},
				{
					title: 'Плавание',
					mod: 'str'
				},
				{
					title: 'Ремесло',
					mod: 'int'
				}
			],
			health: {
  			hpPerLvl: { count: 1, dice: 12 },
  			total: 118,
				current: 118,
				buffed: 0,
			}, // здоровье
			// armorItem: [
			// 	{
			// 		title: 'pojas-telesnogo-sovershenstva',
			// 		name: 'Пояс небесного совершенства',
			// 		bonus: 2,
			// 		plus: [
			// 			{'str': 1},
			// 			{'agl': 1},
			// 			{'stm': 1}
			// 		],
			// 		minus: [],
			// 		active: true,
			// 	},
			// ],
			weaponItem: {
				kosa: {
					title: 'isk-kosa-vampirizma',
					name: 'Искусная коса вампиризма',
					bonus: 2,
					damage: { count: 2, dice: 4 },
					description: 'Восстанавливает половину нанесенного урона',
					active: true,
					multiplier: 1.5,
				}
			}, // снаряженное оружие
			rollResults: null, // результат броска кубика для прередачи в другие компоненты
			historyRoll: [], // история всех роллов todo добавить добавление роллов в историю
			rollType: '', // тип ролла в открытой модалке, нужен в реролле, чтобы понять какой из роллов использовать - для атаки или попадания
			activeTab: 'combat', // дефолтное значение открытой вкладки
			abilities: {
					'Ярость': {
						title: 'Бешеная ярость',
						count: '4 + модификатор выносливости',
						active: false,
						display: true,
						description: 'Сила: +6, Выносливость: +6, Воля: +3, КБ: -2',
						rageGifts: [
							{
								title: 'Могучий удар',
								description: '1 + 1 за каждые 4 уровня персонажа к проверке урона. Может использоваться только один раз за приступ ярости',
								display: true,
								active: false,
							},
							{
								title: 'Восстановление сил',
								description: 'Основное действие. Восстановить 2d8 здоровья один раз в день',
								display: true,
								active: false
							},
							{
								title: 'Устрашающий взгляд',
								description: 'Проверка запугивания врагов на соседней клетке',
								display: false,
								active: false
							},
							{
								title: 'Ужасный вой',
								description: 'Все в радиусе 30 футов должны пройти испытание Воли (10 + 0,5 * лвл + модификатор Силы)',
								display: false,
								active: false
							},
							{
								title:'Снижение урона',
								description: 'Снижение урона на 1',
								display: false,
								active: true,
							},
						],
					},
					'Расовые': {
						'Обнаружение яда': { description: 'Обнаружение яда'},
						'Очищение еды и питья': { description: 'Очищение еды и питья'},
						'Горькоцвет': { description: 'Возможность отращивать светящийся цветок на любой части тела'},
						'Вкусный': { description: 'Штраф к уклонениям после уккуса противником'},
						'Зависимость от света': { description: 'При отсутсвии солнечного света завядаешь'},
						'Семечко': { description: 'Можешь пересадить из себя семечко, умереть и переродиться'},

					}
				},
			features: {
				'Сокрушительный удар': {
					title: 'Сокрушительный удар',
					active: false,
					display: true,
					description: '-3 к попаданию, +5 к урону',
				},
				'Жестокий удар+': {
					title: 'Жестокий удар+',
					active: false,
					display: true,
					description: 'Тройной базовый урон одной атакой',
				},
			},
			hitBonuses: 0,
			damageBonus: 0,
			armoryClass: 30,
			multiplierDamage: 1,
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
				'str': { value: str.base - this._arrSum(str.minus) + this._arrSum(str.plus) + 2, name: str.name, short_name: str.short_name},
				'agl': { value: agl.base - this._arrSum(agl.minus) + this._arrSum(agl.plus) + 2, name: agl.name, short_name: agl.short_name},
				'stm': { value: stm.base - this._arrSum(stm.minus) + this._arrSum(stm.plus) + 2, name: stm.name, short_name: stm.short_name},
				'int': { value: int.base - this._arrSum(int.minus) + this._arrSum(int.plus), name: int.name, short_name: int.short_name},
				'wisdom': { value: wisdom.base - this._arrSum(wisdom.minus) + this._arrSum(wisdom.plus), name: wisdom.name, short_name: wisdom.short_name},
				'char': { value: char.base - this._arrSum(char.minus) + this._arrSum(char.plus), name: char.name, short_name: char.short_name},
			}
		},
		modStatTotal() {
			const { str, agl, stm, int, wisdom, char } = this.mainStatTotal;
			const _getModStat = (stat) => {
				return Math.floor((stat-10)/2);
			};
			// todo переделать в цикл
			return {
				'str': _getModStat(str.value),
				'agl': _getModStat(agl.value),
				'stm': _getModStat(stm.value),
				'int': _getModStat(int.value),
				'wisdom': _getModStat(wisdom.value),
				'char': _getModStat(char.value),
			}
		},
		currentWeapon() {
  		let item = {};
			for (let i in this.weaponItem)
				if(this.weaponItem[i].active === true) {
					item = this.weaponItem[i];
					break;
				}
			return item;
		},
		combatAbilities() {
  		const { title: rageTitle, description: rageDescription, rageGifts, active: rageActive, display: rageDisplay  } =  this.abilities['Ярость'];
  		const { title: crushingBonkTitle, description: crushingBonkDescription, active: crushingBonkActive, display: crushingBonkDisplay  } =  this.features['Сокрушительный удар'];
  		const { title: cruelBonkTitle, description: cruelBonkDescription, active: cruelBonkActive, display: cruelBonkDisplay  } =  this.features['Жестокий удар+'];
			return [
					...rageGifts,
					{ title: rageTitle, description: rageDescription, active: rageActive, display: rageDisplay },
					{ title: crushingBonkTitle, description: crushingBonkDescription, active: crushingBonkActive, display: crushingBonkDisplay },
					{ title: cruelBonkTitle, description: cruelBonkDescription, active: cruelBonkActive, display: cruelBonkDisplay }
					].filter(ability => ability.display).reverse();
		},
	},
	methods: {
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
			const mod = this.BMA[this.currentAttack] + this.modStatTotal.str + this.hitBonuses;
			return {
				value: roll.total + mod,
				rolls: roll.rolls,
				type: (roll.total === (20 || 1) ? 'crit' : 'normal'),
				formula: `Результат кубика: ${ roll.total }<br>
									Модификатор силы: ${ this.modStatTotal.str }<br>
									Текущий БМА: ${ this.BMA[this.currentAttack] }<br>
									Бонус к попаданию: ${ this.hitBonuses }`,
			};
		},
		checkDamage() {
			const damage = this.rollDice(this.currentWeapon.damage.dice,  this.currentWeapon.damage.count * this.multiplierDamage);
			return {
				value: Math.round(this.modStatTotal.str * this.currentWeapon.multiplier) + damage.total + this.damageBonus,
				formula: `Урон от оружия(${this.currentWeapon.damage.count}d${this.currentWeapon.damage.dice}): [${damage.rolls}] = <span style="color: red; font-size: 18px;">${damage.total}</span><br>
									Модификатор силы (${this.modStatTotal.str} * ${this.currentWeapon.multiplier}): <span style="color: red; font-size: 18px;">${Math.round(this.modStatTotal.str * this.currentWeapon.multiplier)}</span><br>
									Бонус урона: ${this.damageBonus}`
			};
		},
		checkHeal() {
			const roll = this.rollDice(8,2);
			return {
				value: roll.total,
				formula: `Броски: ${roll.rolls}`
			};
		},
		rollCheck(type = this.rollType) {
			console.log(type);
			this.rollType = type;
			switch (type) {
				case 'hit': this.rollResults = this.checkHit();
				break;
				case 'damage': this.rollResults = this.checkDamage();
				break;
				case 'heal': this.rollResults = this.checkHeal();
				break;

				default: this.rollResults = this.rollDice();
				break
			}
		},
		clearRollResult() {
			this.historyRoll.push(this.rollResults);
			this.rollResults = null;
			this.rollType = '';
		},
		changeAttack(attack) {
			this.currentAttack = attack;
			console.log('.', attack);
		},
		transferBonuses(bonus) {
			console.log(bonus);
			switch (bonus) {

				case 'Бешеная ярость':
					this.abilities['Ярость'].active = !this.abilities['Ярость'].active;
					if (this.abilities['Ярость'].active) {
						this.mainStat.str.plus.push(6);
						this.mainStat.stm.plus.push(6);
						this.armoryClass -= 2;
						// todo добавить проверку Воли +3
					} else {
						this.mainStat.str.plus.splice(this.mainStat.str.plus.findIndex(item => item === 6), 1);
						this.mainStat.stm.plus.splice(this.mainStat.stm.plus.findIndex(item => item === 6), 1);
					}
				break;

				case 'Могучий удар':
					let bonk = this.abilities['Ярость'].rageGifts.find(gift => gift.title === 'Могучий удар');
					console.log(bonk);
					bonk.active = !bonk.active;

					if(bonk.active) {
						this.damageBonus += 1 + Math.floor(this.currentLvl / 4);
					} else {
						this.damageBonus -= 1 + Math.floor(this.currentLvl / 4);
					}
					break;

				case 'Восстановление сил':
					this.rollCheck('heal');
					break;

				case 'Жестокий удар+':
					this.features['Жестокий удар+'].active = !this.features['Жестокий удар+'].active;
					if (this.features['Жестокий удар+'].active) {
						this.multiplierDamage *= 3;
					} else {
						this.multiplierDamage /= 3;
					}
					break;

				case 'Сокрушительный удар':
					this.features['Сокрушительный удар'].active = !this.features['Сокрушительный удар'].active;
					if (this.features['Сокрушительный удар'].active) {

						this.hitBonuses -= (1 + Math.floor(this.BMA[this.currentAttack] / 4));
						this.damageBonus += 2 * 1.5 +  2 * 1.5 * Math.floor(this.BMA[this.currentAttack] / 4);

					} else {
						this.hitBonuses += (1 + Math.floor(this.BMA[this.currentAttack] / 4));
						this.damageBonus -= 2 * 1.5 +  2 * 1.5 * Math.floor(this.BMA[this.currentAttack] / 4);
					}
				break;
				default:
					break;
			}
		},
	},
}
</script>


<template>
	<div class="character">
<!--		<el-tabs v-model="activeTab">-->
<!--			<el-tab-pane label="Характеристики" name="characteristics">-->
<!--						<characteristic-out :stats="mainStatTotal" :mods="modStatTotal"/>-->
<!--			</el-tab-pane>-->
<!--			<el-tab-pane label="Комбат роллы" name="combat">-->
<!--				<combat :combatAbilities="combatAbilities"-->
<!--								@roll-check="rollCheck"-->
<!--								@transfer-bonuses="transferBonuses"/>-->
<!--			</el-tab-pane>-->
<!--		</el-tabs>-->

		<characteristic-out :stats="mainStatTotal"
												:mods="modStatTotal"
												:armory="armoryClass"/>
		<combat :combatAbilities="combatAbilities"
											@roll-check="rollCheck"
											@transfer-bonuses="transferBonuses"
											@change-attack-number="changeAttack"/>

		<RollResult v-if="rollResults"
								:rollResults="rollResults"
								@reroll="rollCheck"
								@close-dialog="clearRollResult"/>
	</div>
</template>

<style scoped lang="scss">

@media #{"(max-width: 376px)"} {
	.character {
		margin: 35px 50px 0 25px;
		display: flex;
		flex-direction: column;
	}
}
</style>
