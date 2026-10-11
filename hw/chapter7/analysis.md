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
## Q3: Round-Robin and Time Quantum

**Prediction:** I predict that a smaller time quantum will improve response time, while a larger time quantum will reduce average turnaround time and waiting time for this workload.

**Reasoning:** Round-Robin (RR) gives each job a limited amount of CPU time before switching to the next job. A smaller quantum allows jobs to get their first CPU time sooner, but it causes more context switches. A larger quantum reduces switching frequency and makes RR behave more like FIFO.

**Verified Results:**

| Time Quantum | Average Response Time | Average Turnaround Time | Average Waiting Time | Total Time |
|---|---:|---:|---:|---:|
| 1 | 1 | 599 | 399 | 600 |
| 10 | 10 | 590 | 390 | 600 |
| 100 | 100 | 500 | 300 | 600 |

**Analysis:** For three jobs of equal length (200 each), the total time remained 600 for all three quantum values. As the quantum increased from 1 to 100, average response time increased from 1 to 100, while average turnaround time decreased from 599 to 500 and average waiting time decreased from 399 to 300. This shows that a smaller quantum improves initial responsiveness, while a larger quantum reduces waiting and turnaround times for this particular workload. These results may differ when jobs have different lengths.
### Q4. For what types of workloads does SJF deliver the same turnaround times as FIFO?

**Answer:**

SJF delivers the same turnaround times as FIFO when all jobs have the same length. In this case, both scheduling algorithms process the jobs in the same order, so the turnaround times are identical.
### Q5. For what types of workloads and quantum lengths does SJF deliver the same response times as RR?

**Answer:**

SJF and RR can deliver the same response times when all jobs have the same length and the RR time quantum is equal to the length of each job. In this case, each job can finish in one time slice, so both scheduling algorithms give the same response times.

### Q6. What happens to response time with SJF as job lengths increase?

**Prediction:**

I predict that average response time will increase as job lengths increase when all jobs have the same length.

**Reasoning:**

SJF runs the shortest job first. When all jobs have the same length, the jobs run in their original order. Each later job must wait for the previous jobs to finish before receiving CPU time. Therefore, increasing the job lengths increases the response time of later jobs.

**Verified Results:**

The simulator outputs are saved in:
- `data/q6_sjf_20.txt`
- `data/q6_sjf_40.txt`
- `data/q6_sjf_60.txt`

For three equal-length jobs, the expected average response times are:
- Job length 20: average response time = 20
- Job length 40: average response time = 40
- Job length 60: average response time = 60

**Analysis:**

The results demonstrate that average response time increases as job lengths increase. When all jobs have the same length, the first job starts immediately, while later jobs wait for earlier jobs to finish. Longer jobs therefore cause longer waits for the jobs behind them.
### Q7. Convoy Effect

**Prediction:**

I predict that the convoy effect causes short jobs to wait longer when a long job runs first.

**Reasoning:**

The convoy effect occurs when short jobs wait behind a long job. Under FIFO scheduling, jobs run in arrival order, so a short job cannot run until the jobs before it finish.

**Verified Results:**

For job lengths 100, 200, and 300, FIFO runs the jobs in that order. Their response times are 0, 100, and 300, respectively. The average response time is:

Average response time = (0 + 100 + 300) / 3 = 133.33

**Analysis:**

The results show that jobs arriving later must wait for earlier jobs to finish. When a long job is first, short jobs can experience long waiting times. SJF can reduce this problem by running shorter jobs first.