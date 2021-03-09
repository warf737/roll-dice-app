<script>
export default {
	name: 'ToggleAbility',
	data () {
		return {
			abilities: [
				{
					title: 'Сокрушительный удар',
				},
				{
					title: 'Жестокий удар',
				},
				{
					title: 'Бешеная ярость',
				},
				{
					title: 'Могучий удар',
				},
			],
			activeBonuses:[],
		};
	},
	methods: {},
};
</script>

<template>
	<el-checkbox-group v-model="activeBonuses" size="mini">
		<el-checkbox-button v-for="(ability, index) in abilities" :label="ability" :key="index">
			<span>{{ ability.title }}</span>
		</el-checkbox-button>
	</el-checkbox-group>
</template>

<style lang="scss" scoped>
</style>
