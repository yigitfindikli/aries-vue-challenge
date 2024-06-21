<template>
	<div v-if="visible" :style="tooltipStyle" class="tooltip">
		<p>Price: {{ dataPoint.x }}</p>
		<p>Profit/Loss: {{ dataPoint.y.toFixed(2) }}</p>
	</div>
</template>

<script>
export default {
	name: "TooltipComponent",
	props: {
		visible: {
			type: Boolean,
			default: false
		},
		dataPoint: {
			type: Object,
			default: () => ({})
		},
		x: {
			type: Number,
			default: 0
		},
		y: {
			type: Number,
			default: 0
		},
		xSpacing: {
			type: Number,
			default: 14
		},
		ySpacing: {
			type: Number,
			default: 0
		}
	},
	computed: {
		tooltipStyle() {
			return {
				left: `${this.x + this.xSpacing}px`,
				top: `${this.y + this.ySpacing}px`
			};
		}
	}
};
</script>

<style scoped>
.tooltip {
	position: absolute;
	font-size: var(--tooltip-font-size);
	color: var(--tooltip-text-color);
	background-color: var(--tooltip-background-color);
	border: var(--tooltip-border);
	box-shadow: var(--tooltip-box-shadow);
	padding: var(--tooltip-padding);
	border-radius: var(--tooltip-border-radius);
	pointer-events: none;
}
</style>
