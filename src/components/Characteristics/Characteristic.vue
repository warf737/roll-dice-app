<script>
export default {
	name: 'Characteristic',
	props: {
		stat: {
			type: Object,
			default: () => {},
		},
		mod: {
			// type: any,
			// default: () => {},
		}
	},
	methods: {},
};
</script>

<template>
	<div class="item">
		<div class="item__body">
			<p class="item__body-title">{{ stat.short_name }}:</p>
			<p class="item__body-value">{{ stat.value }}</p>
			<p class="item__body-value">{{ mod }}</p>
		</div>
		<div class="item__footer">
<!--			<p>{{ stat.name }}</p>-->
		</div>
	</div>
</template>

<style lang="scss" scoped>
.item {
	display: flex;

	&__body {
		display: flex;
	}

	&__body-title {}
	&__body-value { margin-left: 10px;}
}

</style>
