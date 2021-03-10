<script>
export default {
	name: 'ToggleAbility',
	props: {
		combatAbilities: {
			type: Array,
			default: () => {},
		},
		attackNumber: {
			type: Number,
			default: 0,
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
			if(ability.title === 'Бешеная ярость') {
				return false;
			} else {
				const rageIsActive = this.combatAbilities.find(ability => ability.title === 'Бешеная ярость').active;
				return !rageIsActive;
			}
		},
		changeAbility(ability) {
			this.$emit('change-activity', ability);
		},
		changeAttackNumber() {
			this.$emit('change-attack-number', this.currentAttack);
		}
	},
};
</script>

<template>
<section>
	<el-radio-group v-model="currentAttack">
		<el-radio :label="0">Первая атака</el-radio>
		<el-radio :label="1">Вторая атака</el-radio>
		<el-radio :label="2">Третья атака</el-radio>
	</el-radio-group>

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
</template>

<style lang="scss">

</style>
