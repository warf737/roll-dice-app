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
		armory: {
			type: Number,
			default: 0,
		}
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
		<span class="item">КБ: {{ armory }}</span>
	</ul>
</template>

<style lang="scss" scoped>
.statistic {
		display: flex;
		flex-direction: column;
}
.item {
	margin: 3px 0;
}
</style>
