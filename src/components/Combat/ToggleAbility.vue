<script>
export default {
	name: 'ToggleAbility',
	props: {
		combatAbilities: {
			type: Array,
			default: () => {},
		}
	},
	computed: {
	},
	data () {
		return {
			activeBonuses:[],
		};
	},
	methods: {
		isActive(ability) {
			if(ability.title === 'Бешеная ярость') {
				return false;
			} else {
				const activityRage = this.combatAbilities.find(ability => ability.title === 'Бешеная ярость').active;
				console.log('1', ability.active, '2', activityRage);
				return !(ability.active && activityRage);
			}
		}
	},
};
</script>

<template>
	<el-checkbox-group v-model="activeBonuses" size="mini">
		<el-checkbox-button v-for="(ability, index) in combatAbilities" :label="ability" :key="index" :disabled="isActive(ability)">
			<span>{{ ability.title }}</span>
<!--			<span>{{ ability.description }}</span>-->
		</el-checkbox-button>

	</el-checkbox-group>
</template>

<style lang="scss">
.el-checkbox-group {
	display: flex;
	flex-direction: column;
}
.el-checkbox-button__inner {
	width: 150px;
}
</style>
