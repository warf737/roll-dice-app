<script>
import ToggleAbility from '@/components/Combat/ToggleAbility';

export default {
	name: 'RollResult',
	components: {
		ToggleAbility
	},
	props: {
		rollResults: {
			type: Object,
			default: () => {},
		},
	},
	computed: {
		isVisible() {
			return Boolean(this.rollResults);
		},
	},
	methods: {
		rerollDice() {
			this.$emit('reroll');
		},
		closeDialog() {
			this.$emit('close-dialog');
		},
	},
};
</script>

<template>
	<el-dialog class="roll__wrapper"
						 :visible="isVisible"
						 title="Результат броска"
						 width="30%"
						 @close="closeDialog">
		<span  class="roll__total"
					 :class="{'crit' : rollResults.type === 'crit'}">
			{{ rollResults.value }}
		</span>
		<span class="roll__details"
					v-html="rollResults.formula"/>

		<span slot="footer"
					class="dialog-footer">
    <el-button @click="rerollDice">Reroll</el-button>
    <el-button type="primary"
							 @click="closeDialog">Close</el-button>
  </span>
	</el-dialog>
</template>

<style lang="scss" scoped>

.roll {
	&__total {
		text-align: center;
		font-size: 54px;
		text-shadow: 1px 1px 2px black, 0 0 4em;
	}
}
</style>

<style lang="scss">
.el-dialog__body {
	display: flex;
	flex-direction: column;
}
</style>

<style lang="scss" scoped>
.crit {
	color: red;
	font-size: 72px;
	text-shadow: 1px 1px 2px black, 0 0 10px red;
}
</style>
