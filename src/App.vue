<template>
	<div id="app">
		<div class="responsive-container">
			<h1>Responsive</h1>
			<CodingChallenge :optionsData="sampleData" />
		</div>
		<div>
			<h1>Fixed</h1>
			<CodingChallenge
				:optionsData="sampleData"
				:canvasHeight="400"
				:canvasWidth="600"
				:maintainAspectRatio="false"
			/>
		</div>
		<div>
			<h1>Different Price Range</h1>
			<CodingChallenge
				:optionsData="sampleData"
				:canvasHeight="400"
				:canvasWidth="600"
				:priceRange="[0, 600]"
				:maintainAspectRatio="false"
			/>
		</div>
		<div>
			<h1>fillLossAndProfit False</h1>
			<CodingChallenge
				:optionsData="sampleData"
				:canvasHeight="400"
				:canvasWidth="600"
				:fillLossAndProfit="false"
				:maintainAspectRatio="false"
			/>
		</div>
		<div>
			<h1>Hide Results</h1>
			<CodingChallenge
				:optionsData="sampleData"
				:canvasHeight="400"
				:canvasWidth="600"
				:maintainAspectRatio="false"
				:hideResults="true"
			/>
		</div>
		<div>
			<h1>Custom Label Padding and Point Sizes</h1>
			<CodingChallenge
				:labelPadding="60"
				:optionsData="sampleData"
				:canvasHeight="400"
				:canvasWidth="600"
				:breakEvenPointSize="5"
				:maxProfitPointSize="5"
				:maxLossPointSize="5"
				:maintainAspectRatio="false"
				:hideResults="true"
			/>
		</div>
	</div>
</template>

<script>
import CodingChallenge from "./components/CodingChallenge.vue";
export default {
	name: "App",
	components: {
		CodingChallenge
	},
	data() {
		return {
			sampleData: [
				{
					strike_price: 100,
					type: "Call",
					bid: 10.05,
					ask: 12.04,
					long_short: "long",
					expiration_date: "2025-12-17T00:00:00Z"
				},
				{
					strike_price: 102.5,
					type: "Call",
					bid: 12.1,
					ask: 14,
					long_short: "long",
					expiration_date: "2025-12-17T00:00:00Z"
				},
				{
					strike_price: 103,
					type: "Put",
					bid: 14,
					ask: 15.5,
					long_short: "short",
					expiration_date: "2025-12-17T00:00:00Z"
				},
				{
					strike_price: 105,
					type: "Put",
					bid: 16,
					ask: 18,
					long_short: "long",
					expiration_date: "2025-12-17T00:00:00Z"
				}
			]
		};
	}
};
</script>

<style>
:root {
	/* Light Theme */
	/* --grid-line-color: rgba(0, 0, 0, 0.1);
	--grid-text-color: #303030;
	--grid-text-padding-x: 15;
	--grid-text-padding-y: 35;
	--grid-line-width: 1;
	--grid-font: 14px Arial;
	--axis-color: rgba(0, 0, 0, 0.8);
	--line-stroke-color: #08d97b;
	--line-color: #05a15b;
	--break-even-point-color: #e22b0b;
	--start-point-color: #e22b0b;
	--end-point-color: #08d97b;
	--container-card-border-radius: 14px;
	--canvas-border: 1px solid rgba(0, 0, 0, 0.1);
	--canvas-background-color: rgba(255, 255, 255, 0.3);
	--canvas-box-shadow: 0 1px 2px 0 rgb(0 0 0 / 0.05);
	--canvas-padding: 1rem;
	--line-width: 1;
	--axes-line-width: 1;
	--cursor-line-color: rgba(204, 204, 204, 0.6);
	--cursor-line-width: 1px;
	--cursor-dot-color: rgb(0, 0, 0);
	--profit-area-bg-color: #08d97b28;
	--loss-area-bg-color: #df231d2f;
	--tooltip-font-size: 14px;
	--tooltip-text-color: #000;
	--tooltip-background-color: rgba(255, 255, 255, 0.8);
	--tooltip-border: 1px solid rgba(0, 0, 0, 0.1);
	--tooltip-box-shadow: 0 1px 2px 0 rgb(0 0 0 / 0.05);
	--tooltip-padding: 3.5px;
	--tooltip-border-radius: 7px;

	--background: linear-gradient(-60deg, #ebfdf4 10%, rgb(241, 232, 255) 100%);
	--base-background: #ffffff;
	--text-color: #242424;
	--border-color: rgba(0, 0, 0, 0.1); */

	/* Dark Theme */
	--grid-line-color: rgba(247, 247, 247, 0.1);
	--grid-text-color: #d1d1d1;
	--grid-text-padding-x: 15;
	--grid-text-padding-y: 35;
	--grid-line-width: 1;
	--grid-font: 14px Arial;
	--axis-color: rgba(255, 255, 255, 0.8);
	--line-stroke-color: #08d97b;
	--line-color: #05a15b;
	--break-even-point-color: #e22b0b;
	--start-point-color: #e22b0b;
	--end-point-color: #08d97b;
	--container-spacing: 14px;
	--container-card-border-radius: 14px;
	--canvas-border: 1px solid rgba(255, 255, 255, 0.2);
	--canvas-background-color: rgba(255, 255, 255, 0.12);
	--canvas-box-shadow: 0 1px 2px 0 rgb(0 0 0 / 0.05);
	--canvas-padding: 1rem;
	--line-width: 1;
	--axes-line-width: 1;
	--cursor-line-color: rgba(204, 204, 204, 0.6);
	--cursor-line-width: 1px;
	--cursor-dot-color: rgb(255, 255, 255);
	--profit-area-bg-color: #08d97b3b;
	--loss-area-bg-color: #df571d4b;
	--tooltip-font-size: 14px;
	--tooltip-text-color: #000;
	--tooltip-background-color: rgba(255, 255, 255, 0.8);
	--tooltip-border: 1px solid rgba(255, 255, 255, 0.2);
	--tooltip-box-shadow: 0 1px 2px 0 rgb(0 0 0 / 0.05);
	--tooltip-padding: 3.5px;
	--tooltip-border-radius: 7px;

	--background: linear-gradient(
		-60deg,
		#07c56c67 10%,
		rgba(91, 16, 211, 0.438) 100%
	);
	--base-background: #28323e;
	--text-color: #ffffff;
	--border-color: rgba(255, 255, 255, 0.2);
}

/* For demo purposes */
.responsive-container {
	max-width: 780px;
	min-width: 320px;
	width: 100%;
}

.samples-title {
	margin-top: 4rem;
}
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: var(--text-color);
	background: var(--background);
	background-color: var(--base-background);
	display: flex;
	justify-content: center;
	align-items: center;
	gap: var(--container-spacing);
	flex-direction: column;
	height: 100%;
	padding: var(--container-spacing) 0;
	min-height: 100vh;
}
</style>
