# Flow Playground Performance Analysis

## Test Method

A Playwright-driven interaction test loaded the Flow Playground markup, switched through each level, dragged all tokens into the goal zone, and recorded two metrics per level:

- **Completion time** — the UI timer value after all tokens were placed.
- **Style update loop** — the duration to apply 200 rapid position updates on a token (a proxy for layout responsiveness).

## Results Summary

| Level | Tokens placed | Completion time | Style update loop |
| --- | --- | --- | --- |
| 1 | 3 / 3 | 0.4s | 0.40 ms |
| 2 | 3 / 3 | 0.4s | 0.30 ms |
| 3 | 3 / 3 | 0.4s | 0.40 ms |
| 4 | 3 / 3 | 0.4s | 0.60 ms |

## Performance Analysis

- **Drag responsiveness**: All four levels completed with a 3 / 3 score, indicating consistent token capture and drag handling across layouts.
- **Timer progression**: Each level completed at ~0.4s (automated drag speed), showing the timer updates immediately on level load and stops when goals are met.
- **Style update loop**: The update loop ranged from 0.30–0.60 ms, suggesting low overhead for token repositioning and lightweight DOM updates even when dragging repeatedly.
- **Layout stability**: Tokens remain within board bounds and snap into the goal zone without reflowing the rest of the interface.

Overall, the interface remains responsive under rapid drag interactions, with minimal latency in position updates and consistent behavior across the four level configurations.
