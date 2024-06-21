<template>
	<div class="chart-container">
		<canvas
			ref="chartCanvas"
			:width="dynamicCanvasWidth"
			:height="dynamicCanvasHeight"
		></canvas>
		<TooltipComponent
			:visible="tooltip.visible"
			:dataPoint="tooltip.dataPoint"
			:x="tooltip.x"
			:y="tooltip.y"
		/>
	</div>
</template>

<script>
import TooltipComponent from "./TooltipComponent.vue";
export default {
	name: "ChartComponent",
	components: {
		TooltipComponent
	},
	props: {
		dataPoints: {
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
		maxProfit: {
			type: Number,
			required: true
		},
		maxLoss: {
			type: Number,
			required: true
		},
		breakEvenPoints: {
			type: Array,
			required: true
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
		}
	},
	mounted() {
		this.updateCanvasSize();
		window.addEventListener("resize", this.updateCanvasSize);
		this.$refs.chartCanvas.addEventListener("mousemove", this.showTooltip);
		this.$refs.chartCanvas.addEventListener("mouseleave", this.hideTooltip);
	},
	beforeDestroy() {
		window.removeEventListener("resize", this.updateCanvasSize);
	},
	data() {
		return {
			tooltip: {
				visible: false,
				dataPoint: {},
				x: 0,
				y: 0
			},
			cursorPosition: { x: 0, y: 0, visible: false },
			dynamicCanvasWidth: this.canvasWidth,
			dynamicCanvasHeight: this.canvasHeight
		};
	},
	methods: {
		updateCanvasSize() {
			if (this.maintainAspectRatio) {
				const canvas = this.$refs.chartCanvas;
				const parentWidth = canvas.parentElement.clientWidth;

				const styles = getComputedStyle(canvas);
				const marginHorizontal =
					parseFloat(styles.paddingLeft) +
					parseFloat(styles.paddingRight);
				const borderHorizontal =
					parseFloat(styles.borderLeftWidth) +
					parseFloat(styles.borderRightWidth);

				const newWidth =
					parentWidth - marginHorizontal - borderHorizontal;
				const newHeight = newWidth / this.aspectRatio;

				this.dynamicCanvasWidth = newWidth;
				this.dynamicCanvasHeight = newHeight;

				canvas.width = newWidth;
				canvas.height = newHeight;
			}

			requestAnimationFrame(() => {
				this.drawChart();
			});
		},
		drawCursor(ctx) {
			if (this.cursorPosition.visible) {
				ctx.strokeStyle =
					this.getPropertyValue("--cursor-line-color") || "#ccc";
				ctx.lineWidth =
					parseFloat(
						getComputedStyle(
							document.documentElement
						).getPropertyValue("--cursor-line-width")
					) || 1;
				ctx.beginPath();
				// Vertical line
				ctx.moveTo(this.cursorPosition.x, this.labelPadding);
				ctx.lineTo(
					this.cursorPosition.x,
					this.dynamicCanvasHeight - this.labelPadding
				);
				// Horizontal line
				ctx.moveTo(this.labelPadding, this.cursorPosition.y);
				ctx.lineTo(
					this.dynamicCanvasWidth - this.labelPadding,
					this.cursorPosition.y
				);
				ctx.stroke();

				ctx.fillStyle =
					this.getPropertyValue("--cursor-dot-color") || "black";
				ctx.beginPath();
				ctx.arc(
					this.cursorPosition.x,
					this.cursorPosition.y,
					3,
					0,
					2 * Math.PI
				);
				ctx.fill();
			}
		},
		drawChart() {
			const canvas = this.$refs.chartCanvas;
			const ctx = canvas.getContext("2d");

			// Clear the canvas
			ctx.clearRect(0, 0, canvas.width, canvas.height);

			// Calculate the min and max values for scaling
			const xValues = this.dataPoints.map((point) => point.x);
			const yValues = this.dataPoints.map((point) => point.y);
			const minX = Math.min(...xValues);
			const maxX = Math.max(...xValues);
			const minY = Math.min(...yValues);
			const maxY = Math.max(...yValues);

			// Define padding and scaling factors
			const padding = this.labelPadding;
			const xScale = (canvas.width - 2 * padding) / (maxX - minX);
			const yScale = (canvas.height - 2 * padding) / (maxY - minY);

			// Helper function to map data points to canvas coordinates
			const mapToCanvas = (x, y) => ({
				x: (x - minX) * xScale + padding,
				y: canvas.height - (y - minY) * yScale - padding
			});

			this.drawGrid(
				ctx,
				minX,
				maxX,
				minY,
				maxY,
				mapToCanvas,
				padding,
				canvas
			);
			this.drawAxes(ctx, mapToCanvas, padding, canvas);

			if (this.fillLossAndProfit)
				this.drawProfitLossAreas(ctx, mapToCanvas, padding, canvas);

			this.drawDataPoints(ctx, mapToCanvas);
			this.drawBreakEvenPoints(ctx, mapToCanvas);
			this.drawCursor(ctx);
		},
		drawGrid(ctx, minX, maxX, minY, maxY, mapToCanvas, padding, canvas) {
			// Draw grid lines
			ctx.strokeStyle =
				this.getPropertyValue("--grid-line-color") || "#e0e0e0";
			ctx.lineWidth = this.getPropertyValue("--grid-line-width") || 1;
			ctx.font = this.getPropertyValue("--grid-font") || "12px Arial";
			ctx.fillStyle =
				this.getPropertyValue("--grid-text-color") || "#000";
			const textPaddingX =
				parseFloat(this.getPropertyValue("--text-padding-x")) || 15;
			const textPaddingY =
				parseFloat(this.getPropertyValue("--text-padding-y")) || 35;

			ctx.beginPath();
			for (let i = minX; i <= maxX; i += (maxX - minX) / 10) {
				const { x } = mapToCanvas(i, 0);
				ctx.moveTo(x, padding);
				ctx.lineTo(x, canvas.height - padding);
				ctx.fillText(
					i.toFixed(0),
					x,
					canvas.height - padding + textPaddingX
				);
			}
			for (let i = minY; i <= maxY; i += (maxY - minY) / 10) {
				const { y } = mapToCanvas(0, i);
				ctx.moveTo(padding, y);
				ctx.lineTo(canvas.width - padding, y);
				ctx.fillText(i.toFixed(0), padding - textPaddingY, y + 3);
			}
			ctx.stroke();
		},
		drawAxes(ctx, mapToCanvas, padding, canvas) {
			// Draw axes
			ctx.strokeStyle = this.getPropertyValue("--axis-color") || "#000";
			ctx.lineWidth = this.getPropertyValue("--axes-line-width") || 2;
			ctx.beginPath();
			const { x: zeroX, y: zeroY } = mapToCanvas(0, 0);
			ctx.moveTo(padding, zeroY);
			ctx.lineTo(canvas.width - padding, zeroY);
			ctx.moveTo(zeroX, padding);
			ctx.lineTo(zeroX, canvas.height - padding);
			ctx.stroke();
		},
		drawDataPoints(ctx, mapToCanvas) {
			// Draw data points and connecting lines
			ctx.strokeStyle =
				this.getPropertyValue("--line-stroke-color") || "blue";
			ctx.fillStyle = this.getPropertyValue("--line-color") || "blue";
			ctx.lineWidth =
				parseFloat(this.getPropertyValue("--line-width")) || 3;
			ctx.beginPath();
			this.dataPoints.forEach((point, index) => {
				const { x, y } = mapToCanvas(point.x, point.y);
				if (index === 0) {
					ctx.moveTo(x, y);
				} else {
					ctx.lineTo(x, y);
				}
			});
			ctx.stroke();

			// Draw data points
			this.dataPoints.forEach((point) => {
				const { x, y } = mapToCanvas(point.x, point.y);
				ctx.beginPath();
				ctx.arc(x, y, this.dataPointSize, 0, 2 * Math.PI);
				ctx.fill();
			});
		},
		drawBreakEvenPoints(ctx, mapToCanvas) {
			ctx.fillStyle =
				this.getPropertyValue("--break-even-point-color") || "red";
			this.breakEvenPoints.forEach((point) => {
				const { x, y } = mapToCanvas(point, 0);
				ctx.beginPath();
				ctx.arc(x, y, this.breakEvenPointSize, 0, 2 * Math.PI);
				ctx.fill();
			});

			// Draw max profit and max loss points
			const maxProfitCoords = mapToCanvas(
				this.dataPoints[this.dataPoints.length - 1].x,
				this.maxProfit
			);
			const maxLossCoords = mapToCanvas(
				this.dataPoints[0].x,
				this.maxLoss
			);

			ctx.fillStyle =
				this.getPropertyValue("--end-point-color") || "green";
			ctx.beginPath();
			ctx.arc(
				maxProfitCoords.x,
				maxProfitCoords.y,
				this.maxProfitPointSize,
				0,
				2 * Math.PI
			);
			ctx.fill();

			ctx.fillStyle =
				this.getPropertyValue("--start-point-color") || "green";
			ctx.beginPath();
			ctx.arc(
				maxLossCoords.x,
				maxLossCoords.y,
				this.maxLossPointSize,
				0,
				2 * Math.PI
			);
			ctx.fill();
		},
		drawProfitLossAreas(ctx, mapToCanvas, padding, canvas) {
			const zeroY = mapToCanvas(0, 0).y;

			// Draw profit area
			ctx.fillStyle =
				this.getPropertyValue("--profit-area-bg-color") ||
				"rgba(0, 255, 0, 0.1)"; // Light green for profit
			ctx.beginPath();
			ctx.moveTo(padding, zeroY);
			this.dataPoints.forEach((point) => {
				const { x, y } = mapToCanvas(point.x, Math.max(point.y, 0));
				ctx.lineTo(x, y);
			});
			ctx.lineTo(canvas.width - padding, zeroY);
			ctx.closePath();
			ctx.fill();

			// Draw loss area
			ctx.fillStyle =
				this.getPropertyValue("--loss-area-bg-color") ||
				"rgba(255, 0, 0, 0.1)"; // Light red for loss
			ctx.beginPath();
			ctx.moveTo(padding, zeroY);
			this.dataPoints.forEach((point) => {
				const { x, y } = mapToCanvas(point.x, Math.min(point.y, 0));
				ctx.lineTo(x, y);
			});
			ctx.lineTo(canvas.width - padding, zeroY);
			ctx.closePath();
			ctx.fill();
		},
		getPropertyValue(value) {
			return getComputedStyle(document.documentElement).getPropertyValue(
				value
			);
		},
		showTooltip(event) {
			const canvas = this.$refs.chartCanvas;
			const rect = canvas.getBoundingClientRect();
			const x = event.clientX - rect.left;

			const xValues = this.dataPoints.map((point) => point.x);
			const yValues = this.dataPoints.map((point) => point.y);
			const minX = Math.min(...xValues);
			const maxX = Math.max(...xValues);
			const minY = Math.min(...yValues);
			const maxY = Math.max(...yValues);

			const padding = this.labelPadding;
			const xScale = (canvas.width - 2 * padding) / (maxX - minX);
			const yScale = (canvas.height - 2 * padding) / (maxY - minY);

			const mappedX = (x - padding) / xScale + minX;

			const mapToCanvas = (x, y) => ({
				x: (x - minX) * xScale + padding,
				y: canvas.height - (y - minY) * yScale - padding
			});

			const closestPoint = this.dataPoints.reduce((prev, curr) =>
				Math.abs(curr.x - mappedX) < Math.abs(prev.x - mappedX)
					? curr
					: prev
			);

			this.tooltip.visible = true;
			this.tooltip.dataPoint = closestPoint;
			this.tooltip.x = event.clientX + window.scrollX;
			this.tooltip.y = event.clientY + window.scrollY - this.labelPadding;

			this.cursorPosition.x = mapToCanvas(
				closestPoint.x,
				closestPoint.y
			).x;
			this.cursorPosition.y = mapToCanvas(
				closestPoint.x,
				closestPoint.y
			).y;
			this.cursorPosition.visible = true;

			this.drawChart();
		},
		hideTooltip() {
			this.tooltip.visible = false;
			this.cursorPosition.visible = false;
			this.drawChart();
		}
	}
};
</script>

<style scoped>
canvas {
	border: var(--canvas-border);
	box-shadow: var(--canvas-box-shadow);
	background-color: var(--canvas-background-color);
	padding: var(--canvas-padding);
	border-radius: var(--container-card-border-radius);
}
</style>
