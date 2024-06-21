<template>
	<div v-if="results" class="challenge-container">
		<ChartComponent
			:dataPoints="graphData"
			:canvasWidth="canvasWidth"
			:canvasHeight="canvasHeight"
			:maxProfit="maxProfit"
			:maxLoss="maxLoss"
			:breakEvenPoints="breakEvenPoints"
			v-bind="$props"
		/>
		<div v-if="!hideResults" class="results-container">
			<h2>Results</h2>
			<p>Max Profit: {{ maxProfit.toFixed(2) }}</p>
			<p>Max Loss: {{ maxLoss.toFixed(2) }}</p>
			<p>
				Break-Even Points:
				{{
					breakEvenPoints.map((point) => point.toFixed(2)).join(", ")
				}}
			</p>
		</div>
	</div>
</template>

<script>
import ChartComponent from "./ChartComponent.vue";

export default {
	name: "CodingChallenge",
	components: {
		ChartComponent
	},
	props: {
		optionsData: {
			type: Array,
			required: true
		},
		canvasWidth: {
			type: Number,
			default: 600
		},
		canvasHeight: {
			type: Number,
			default: 400
		},
		priceRange: {
			type: Array,
			default: () => [0, 400]
		},
		dataPointSize: {
			type: Number,
			default: 1
		},
		breakEvenPointSize: {
			type: Number,
			default: 3
		},
		maxProfitPointSize: {
			type: Number,
			default: 3
		},
		maxLossPointSize: {
			type: Number,
			default: 3
		},
		labelPadding: {
			type: Number,
			default: 40
		},
		fillLossAndProfit: {
			type: Boolean,
			default: true
		},
		maintainAspectRatio: {
			type: Boolean,
			default: true
		},
		aspectRatio: {
			type: Number,
			default: 16 / 9
		},
		hideResults: {
			type: Boolean,
			default: false
		}
	},
	data() {
		return {
			results: null,
			prices: [],
			profits: [],
			graphData: []
		};
	},
	mounted() {
		this.calculate();
	},
	methods: {
		calculate() {
			const [minPrice, maxPrice] = this.priceRange;
			this.prices = Array.from(
				{ length: maxPrice - minPrice + 1 },
				(_, i) => i + minPrice
			);
			this.profits = this.prices.map((price) =>
				this.calculateProfit(price)
			);
			this.graphData = this.prices.map((price, index) => ({
				x: price,
				y: this.profits[index]
			}));
			this.calculateResults(this.profits, this.prices);
		},
		calculateProfit(price) {
			let totalProfit = 0;
			this.optionsData.forEach((option) => {
				let intrinsicValue = 0;
				if (option.type === "Call") {
					intrinsicValue = Math.max(0, price - option.strike_price);
				} else {
					intrinsicValue = Math.max(0, option.strike_price - price);
				}
				const premium = (option.bid + option.ask) / 2;
				const position = option.long_short === "long" ? 1 : -1;
				const optionProfit = (intrinsicValue - premium) * position;
				totalProfit += optionProfit;
			});
			return totalProfit;
		},
		calculateResults(profits, prices) {
			const maxProfitValue = Math.max(...profits);
			const maxLossValue = Math.min(...profits);
			const breakEvenPointsValue = [];
			for (let i = 1; i < prices.length; i++) {
				if (
					(profits[i - 1] < 0 && profits[i] > 0) ||
					(profits[i - 1] > 0 && profits[i] < 0) ||
					profits[i] === 0
				) {
					if (profits[i] === 0) {
						breakEvenPointsValue.push(prices[i]);
					} else {
						const breakEvenPrice =
							prices[i - 1] +
							((prices[i] - prices[i - 1]) *
								(0 - profits[i - 1])) /
								(profits[i] - profits[i - 1]);
						breakEvenPointsValue.push(breakEvenPrice);
					}
				}
			}
			this.results = {
				maxProfit: maxProfitValue,
				maxLoss: maxLossValue,
				breakEvenPoints: breakEvenPointsValue
			};
		}
	},
	computed: {
		maxProfit() {
			return this.results ? this.results.maxProfit : 0;
		},
		maxLoss() {
			return this.results ? this.results.maxLoss : 0;
		},
		breakEvenPoints() {
			return this.results ? this.results.breakEvenPoints : [];
		}
	}
};
</script>

<style scoped>
.challenge-container {
	width: 100%;
}

.results-container {
	border: 1px solid var(--border-color);
	margin-top: var(--container-spacing);
	background-color: var(--canvas-background-color);
	backdrop-filter: blur(10px);
	border-radius: var(--container-card-border-radius);
}
</style>
