# Chapter 7: CPU Scheduling

## Q1: FIFO and SJF Comparison

### Prediction
FIFO runs jobs in the order they arrive. SJF runs the shortest job first.

### Reasoning
For job lengths 100, 200, and 300, FIFO completes jobs in that order. Since the jobs are already ordered from shortest to longest, SJF should produce the same execution order.

### Verified Results
The simulator outputs are saved in:
- `data/q1_fifo.txt`
- `data/q1_sjf.txt`

### Analysis
Because the jobs are listed in increasing order of length, FIFO and SJF should have the same response times and turnaround times for this workload.

