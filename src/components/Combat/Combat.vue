<script>
import ToggleAbility from '@/components/Combat/ToggleAbility';

export default {
	name: 'Combat.vue',
	components: {
		ToggleAbility
	},
	data () {
		return {};
	},
	methods: {
		rollCheck(type) {
			this.$emit('roll-check', type);
		}
	},
};
</script>

<template>
	<div>
		<el-button @click="rollCheck('hit')">Проверка на попадание</el-button>
		<el-button @click="rollCheck('damage')">Нанесение урона</el-button>
		<ToggleAbility/>
	</div>
</template>

<style lang="scss" scoped>
</style>
