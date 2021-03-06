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
		}
	},
};
</script>

<template>
	<ul class="statistic">
		<li class="statistic__item"
				v-for="(stat, index) in stats"
				:key="index">
			<characteristic :stat="stat"/>
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
