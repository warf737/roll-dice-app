<script>


export default {
	name: 'Combat',
	components: {
	},
	props: {
		combatAbilities: {
			type: Array,
			default: () => {},
		},
	},
	data () {
		return {
			activeBonuses:[],
			currentAttack: 0,
		};
	},
	methods: {
		isDisabled(ability) {
			if (ability.title === 'Бешеная ярость' ||
					ability.title ==='Сокрушительный удар' ||
					ability.title ==='Жестокий удар+') {
				return false;
			} else {
				const rageIsActive = this.combatAbilities.find(ability => ability.title === 'Бешеная ярость').active;
				return !rageIsActive;
			}
		},
		changeAbility(ability) {
			this.$emit('transfer-bonuses', ability);
		},
		changeAttackNumber() {
			this.$emit('change-attack-number', this.currentAttack);
		},
		rollCheck(type) {
			this.$emit('roll-check', type);
		},
	},
};
</script>

<template>
	<div>
		<el-button @click="rollCheck('hit')">Проверка на попадание</el-button>
		<el-button @click="rollCheck('damage')">Нанесение урона</el-button>

		<section>
			<div class="combat__bma-wrapper" @change="changeAttackNumber">
			<span>БМА: </span>
			<el-radio-group v-model="currentAttack" >
				<el-radio :label="0" border size="small">11</el-radio>
				<el-radio :label="1" border size="small">6</el-radio>
				<el-radio :label="2" border size="small">1</el-radio>
			</el-radio-group>
			</div>

			<el-checkbox-group v-model="activeBonuses" size="mini">
				<el-checkbox-button v-for="(ability, index) in combatAbilities"
														:label="ability.title"
														:key="index"
														:disabled="isDisabled(ability)"
														@change="changeAbility(ability.title)">
					<span>{{ ability.title }}</span>
				</el-checkbox-button>
			</el-checkbox-group>
		</section>
	</div>
</template>

<style lang="scss">
.el-checkbox-group {
	display: flex;
	flex-direction: column;
	width: 150px;
}
.el-checkbox-button__inner {
	width: 150px;
}

.el-radio-group {

	.el-radio {
		margin-right: auto;
	}
	.el-radio__label {
		padding-left: 4px;
	}
}
</style>

<style scoped lang="scss">
.combat {
	&__bma-wrapper {
		display: flex;
		flex-direction: column;
		width: 250px;
	}
}
</style>
