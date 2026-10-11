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

## Q2: FIFO and SJF with Different Job Lengths

### Prediction
I predict that SJF will perform better than FIFO when the jobs are not listed from shortest to longest.

### Reasoning
FIFO runs jobs in their arrival order. SJF runs the shortest job first, so shorter jobs may wait less time.

### Verified Results
For job lengths 300, 100, and 200:

- FIFO average response time: 233.33
- FIFO average turnaround time: 433.33
- FIFO average wait time: 233.33
- SJF average response time: 133.33
- SJF average turnaround time: 333.33
- SJF average wait time: 133.33

The raw outputs are saved in `data/q2_fifo.txt` and `data/q2_sjf.txt`.

### Analysis
SJF performed better for this workload. It ran the 100-second job first, followed by the 200-second job and then the 300-second job. Compared with FIFO, SJF reduced all three average metrics by 100 seconds.
