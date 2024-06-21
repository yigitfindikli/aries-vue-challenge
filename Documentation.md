### Props

| Prop Name             | Type    | Default  | Description                                                         |
| --------------------- | ------- | -------- | ------------------------------------------------------------------- |
| `dataPoints`          | Array   | Required | The data points to be plotted on the chart.                         |
| `canvasWidth`         | Number  | 600      | The width of the canvas.                                            |
| `canvasHeight`        | Number  | 400      | The height of the canvas.                                           |
| `maxProfit`           | Number  | Required | The maximum profit value.                                           |
| `maxLoss`             | Number  | Required | The maximum loss value.                                             |
| `breakEvenPoints`     | Array   | Required | The break-even points to be plotted on the chart.                   |
| `dataPointSize`       | Number  | 1        | The size of the data points.                                        |
| `breakEvenPointSize`  | Number  | 3        | The size of the break-even points.                                  |
| `maxProfitPointSize`  | Number  | 3        | The size of the max profit points.                                  |
| `maxLossPointSize`    | Number  | 3        | The size of the max loss points.                                    |
| `labelPadding`        | Number  | 40       | The padding for the labels on the axes.                             |
| `fillLossAndProfit`   | Boolean | true     | Whether to fill the areas representing loss and profit.             |
| `maintainAspectRatio` | Boolean | true     | Whether to maintain the aspect ratio of the canvas during resizing. |
| `aspectRatio`         | Number  | 16/9     | The aspect ratio to maintain when resizing the canvas.              |

### CSS Variables

| Variable Name                    | Default Value                                                                                                                                            | Description                                      |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| `--grid-line-color`              | `rgba(0, 0, 0, 0.1)` (Light) / `rgba(247, 247, 247, 0.1)` (Dark)                                                                                         | Color of the grid lines.                         |
| `--grid-text-color`              | `#303030` (Light) / `#d1d1d1` (Dark)                                                                                                                     | Color of the grid text.                          |
| `--grid-text-padding-x`          | `15`                                                                                                                                                     | Horizontal padding for the grid text.            |
| `--grid-text-padding-y`          | `35`                                                                                                                                                     | Vertical padding for the grid text.              |
| `--grid-line-width`              | `1`                                                                                                                                                      | Width of the grid lines.                         |
| `--grid-font`                    | `14px Arial`                                                                                                                                             | Font used for the grid text.                     |
| `--axis-color`                   | `rgba(0, 0, 0, 0.8)` (Light) / `rgba(255, 255, 255, 0.8)` (Dark)                                                                                         | Color of the axes lines.                         |
| `--line-stroke-color`            | `#08d97b`                                                                                                                                                | Stroke color of the line connecting data points. |
| `--line-color`                   | `#05a15b`                                                                                                                                                | Color of the data points.                        |
| `--break-even-point-color`       | `#e22b0b`                                                                                                                                                | Color of the break-even points.                  |
| `--start-point-color`            | `#e22b0b`                                                                                                                                                | Color of the starting data point.                |
| `--end-point-color`              | `#08d97b`                                                                                                                                                | Color of the ending data point.                  |
| `--container-card-border-radius` | `14px`                                                                                                                                                   | Border radius of the canvas container.           |
| `--canvas-border`                | `1px solid rgba(0, 0, 0, 0.1)` (Light) / `1px solid rgba(255, 255, 255, 0.2)` (Dark)                                                                     | Border style of the canvas.                      |
| `--canvas-background-color`      | `rgba(255, 255, 255, 0.3)` (Light) / `rgba(255, 255, 255, 0.12)` (Dark)                                                                                  | Background color of the canvas.                  |
| `--canvas-box-shadow`            | `0 1px 2px 0 rgb(0 0 0 / 0.05)`                                                                                                                          | Box shadow of the canvas.                        |
| `--canvas-padding`               | `1rem`                                                                                                                                                   | Padding inside the canvas.                       |
| `--line-width`                   | `1`                                                                                                                                                      | Width of the line connecting data points.        |
| `--axes-line-width`              | `1`                                                                                                                                                      | Width of the axes lines.                         |
| `--cursor-line-color`            | `rgba(204, 204, 204, 0.6)`                                                                                                                               | Color of the cursor lines.                       |
| `--cursor-line-width`            | `1px`                                                                                                                                                    | Width of the cursor lines.                       |
| `--cursor-dot-color`             | `rgb(0, 0, 0)` (Light) / `rgb(255, 255, 255)` (Dark)                                                                                                     | Color of the cursor dot.                         |
| `--profit-area-bg-color`         | `#08d97b28` (Light) / `#08d97b3b` (Dark)                                                                                                                 | Background color for the profit area.            |
| `--loss-area-bg-color`           | `#df231d2f` (Light) / `#df571d4b` (Dark)                                                                                                                 | Background color for the loss area.              |
| `--tooltip-font-size`            | `14px`                                                                                                                                                   | Font size for the tooltip text.                  |
| `--tooltip-text-color`           | `#000`                                                                                                                                                   | Text color for the tooltip.                      |
| `--tooltip-background-color`     | `rgba(255, 255, 255, 0.8)`                                                                                                                               | Background color for the tooltip.                |
| `--tooltip-border`               | `1px solid rgba(0, 0, 0, 0.1)` (Light) / `1px solid rgba(255, 255, 255, 0.2)` (Dark)                                                                     | Border style of the tooltip.                     |
| `--tooltip-box-shadow`           | `0 1px 2px 0 rgb(0 0 0 / 0.05)`                                                                                                                          | Box shadow of the tooltip.                       |
| `--tooltip-padding`              | `3.5px`                                                                                                                                                  | Padding inside the tooltip.                      |
| `--tooltip-border-radius`        | `7px`                                                                                                                                                    | Border radius of the tooltip.                    |
| `--background`                   | `linear-gradient(-60deg, #ebfdf4 10%, rgb(241, 232, 255) 100%)` (Light) / `linear-gradient(-60deg, #07c56c67 10%, rgba(91, 16, 211, 0.438) 100%)` (Dark) | Background gradient.                             |
| `--base-background`              | `#ffffff` (Light) / `#28323e` (Dark)                                                                                                                     | Base background color.                           |
| `--text-color`                   | `#242424` (Light) / `#ffffff` (Dark)                                                                                                                     | Base text color.                                 |
| `--border-color`                 | `rgba(0, 0, 0, 0.1)` (Light) / `rgba(255, 255, 255, 0.2)` (Dark)                                                                                         | Base border color.                               |
