# OSTEP Ch.4 Homework Answers

## Q1
- Prediction / 预测:CPU 会空闲。
- Reasoning / 理由:我认为两个进程虽然都只使用 CPU，但在进程切换时可能会出现 CPU 空闲。
- Verified result / 验证结果:Total Time = 10，CPU Busy = 100.00%，IO Busy = 0.00%。
- Analysis / 分析:我的预测错误了。两个进程都只使用 CPU，所以当 PID 0 运行时，PID 1 处于 READY；PID 0 完成后，PID 1 立即使用 CPU。因此 CPU 在整个 10 个 tick 中都没有空闲。

## Q2
- Prediction / 预测:PID 1 会进行 I/O，并进入 BLOCKED 状态。
- Reasoning / 理由:因为 PID 1 的设置是 1:0，表示它只有一条指令，而且这条指令是 I/O。
- Verified result / 验证结果:Total Time = 11，CPU Busy = 54.55%，IO Busy = 45.45%。
- Analysis / 分析:我的预测正确。PID 0 先使用 CPU 4 个 tick。第 5 个 tick，PID 1 发起 I/O；之后阻塞 5 个 tick，第 11 个 tick 处理 I/O 完成。一次 I/O 总共需要 7 个 tick：1 个 tick 发起 I/O、5 个 tick BLOCKED、1 个 tick 处理完成。

## Q3
- Prediction / 预测:CPU 会一直等待 PID 0 的 I/O。
- Reasoning / 理由:我认为 PID 0 正在进行 I/O 并等待完成，所以 CPU 可能会等待它。
- Verified result / 验证结果:Total Time = 7，CPU Busy = 85.71%，IO Busy = 71.43%。
- Analysis / 分析:我的预测错误了。PID 0 进入 BLOCKED 状态等待 I/O 时，PID 1 可以使用 CPU 执行自己的 4 条指令。因此 CPU 没有一直等待，而是继续工作。

## Q4
- Prediction / 预测: CPU 仍然会运行 PID 1。
- Reasoning / 理由:我认为虽然 PID 0 在等待 I/O，但 PID 1 仍然可以使用 CPU。
- Verified result / 验证结果:Total Time = 11，CPU Busy = 54.55%，IO Busy = 45.45%。
- Analysis / 分析:我的预测错误了。在 SWITCH_ON_END 模式下，PID 0 发起 I/O 后进入 BLOCKED，但系统不会在 I/O 等待期间切换到 PID 1。因此 PID 1 一直处于 READY，CPU 在这段时间空闲，直到 PID 0 的 I/O 完成后才运行 PID 1。

## Q5
- Prediction / 预测:Total Time 会变短，小于 11。
- Reasoning / 理由: SWITCH_ON_IO 会在 PID 0 进入 I/O 阻塞后切换到 PID 1，让 PID 1 利用 CPU，减少 CPU 空闲时间。
- Verified result / 验证结果: Total Time = 7，CPU Busy = 85.71%，IO Busy = 71.43%。
- Analysis / 分析:我的预测正确。与 SWITCH_ON_END 不同，SWITCH_ON_IO 会在 PID 0 进入 BLOCKED 后立即切换到 PID 1。因此 PID 1 可以利用原本 CPU 空闲的时间完成 4 条 CPU 指令，使总时间从 11 个 tick 缩短到 7 个 tick。

## Q6
- Prediction / 预测:PID 0 的 I/O 完成后，会等当前运行的进程结束后再运行。
- Reasoning / 理由: 因为设置了 IO_RUN_LATER，所以我认为 I/O 完成后 PID 0 不会立即重新获得 CPU。
- Verified result / 验证结果:Total Time = 31，CPU Busy = 67.74%，IO Busy = 48.39%。
- Analysis / 分析: IO_RUN_LATER 不会让 I/O 完成的进程立即抢占 CPU。I/O 完成后，PID 0 会等待合适的调度时机再运行。实验结果显示，PID 0 在 Time 24 完成 I/O，并在 Time 25 开始下一次 I/O，说明它没有因为 I/O 完成而立即抢占其他进程。

## Q7
- Prediction / 预测:可以更快地继续下一次 I/O，减少 I/O 设备空闲的时间。
- Reasoning / 理由: I/O 完成后立即让 PID 0 运行，可以让这个 I/O 密集型进程更快开始下一次 I/O，从而提高 I/O 设备的利用率。
- Verified result / 验证结果: Total Time = 21，CPU Busy = 100.00%，IO Busy = 71.43%。
- Analysis / 分析: 我的预测正确。使用 IO_RUN_IMMEDIATE 时，PID 0 的 I/O 完成后会立即再次运行并开始下一次 I/O。这样可以减少 I/O 设备的空闲时间，并且 CPU 也能够在其他进程运行时保持忙碌。

## Q8
- Prediction / 预测:我预测 IO_RUN_IMMEDIATE 会比默认设置更快，而 SWITCH_ON_END 会比默认设置花费更多时间。
- Reasoning / 理由:因为 IO_RUN_IMMEDIATE 会在 I/O 完成后立即让对应进程继续运行，可以减少等待时间；而 SWITCH_ON_END 会让进程等待当前运行的进程结束后才能运行，因此可能增加总运行时间。
- Verified result / 验证结果:
Seed 1：默认 Total Time = 15，IO_RUN_IMMEDIATE = 15，SWITCH_ON_END = 18。
Seed 2：默认 Total Time = 16，IO_RUN_IMMEDIATE = 16，SWITCH_ON_END = 30。
Seed 3：默认 Total Time = 18，IO_RUN_IMMEDIATE = 17，SWITCH_ON_END = 24。
- Analysis / 分析:实验结果基本符合我的预测。SWITCH_ON_END 在三个 seed 中的 Total Time 都比默认设置更长，说明等待当前进程结束会增加运行时间。IO_RUN_IMMEDIATE 与默认设置相比，Seed 1 和 Seed 2 的结果相同，Seed 3 的 Total Time 从 18 降低到了 17。因此，IO_RUN_IMMEDIATE 可以在某些情况下减少等待，而 SWITCH_ON_END 会明显增加等待时间。