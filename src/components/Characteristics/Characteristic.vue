<script>
export default {
	name: 'Characteristic',
	props: {
		stat: {
			type: Object,
			default: () => {},
		}
	},
	methods: {},
};
</script>

<template>
	<div class="item">
		<div class="item__body">
			<p>{{ stat.short_name }}:</p>
			<p >{{ stat.value }}</p>
		</div>
		<div class="item__footer">
			<p>{{ stat.name }}</p>
		</div>
	</div>
</template>

<style lang="scss" scoped>
</style>
