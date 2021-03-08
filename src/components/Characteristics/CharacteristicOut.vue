<script>
import characteristic from '@/components/Characteristics/Characteristic';
export default {
	name: 'CharacteristicOut',
	components: {
		characteristic,
	},
	props: {
		stats: {
			type: Object,
			default: () => {},
		},
		mods: {
			type: Object,
			default: () => {},
		},
	},
};
</script>

<template>
	<ul class="statistic">
		<li class="statistic__item"
				v-for="(value, name, index) in stats"
				:key="index">
			<characteristic :stat="value" :mod="mods[name]"/>
		</li>
	</ul>
</template>

<style lang="scss" scoped>
.statistic {
		display: flex;
		flex-direction: column;
	&__item {
		//display: flex;
	}
}
</style>
