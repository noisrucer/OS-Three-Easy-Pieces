> (Q1) Run `process-run.py` with the following flags: `-l 5:100, 5:100`. What should the CPU utilization be?

Process 0 takes `50%` and Process 1 takes `50%`. While one process is running, the other is put in "READY" state.

> (Q2) Now run with these flags `-l 4:100,1:0`. How long does it takes to complete both processes?

It takes 11 in total (4 RUN:CPU + 1 RUN:IO + 5 WAITING + 1 RUN:i_done)

> (Q3) Switch the order of the processes: `-l 1:0,4:100`. What happens?

Total 7: This time, while the Process 0 is in "BLOCKED" or "WAITING" state due to I/O, Process 1 is assigned to the CPU and completes the 4 instructions. Hence, it's more efficien than `Q2`.

> (Q4) `-l 1:0,4:100 -c -S SWITCH_ON_END`

Process 0 initiates I/O and Process 1 is in BLOCKED state until the Process 0 is done.

> (Q5) `-l 1:0,4:100 -c -S SWITCH_ON_IO`

Unlike `SWITCH_ON_END`, `SWITCH_ON_IO` enables the system to switch to Process 1 while Process 0 is waiting for I/O to complete. Hence, it works the same way as (Q3).

> (Q6) `-l 3:0,5:100,5:100,5:100 -S SWITCH_ON_IO -I IO_RUN_LATER -c -p`

Total 31 time units. When Process O completes its I/O, Process 1, 2, 3 start running, respectively in order. Meanwhile, Process 0 waits for them before it starts the remaining I/Os. This is ineffective since if Process 0 ran immediately after the first I/O, then while the Process 0 is waiting for I/O completion, other processes could do their works concurrently.

> (Q7) `-l 3:0,5:100,5:100,5:100 -S SWITCH_ON_IO -I IO_RUN_IMMEDIATE -c -p`

Total 21 time units. After each I/O completion of Process 0, it starts the next I/O immediately so other processes run concurrently while Process 0 is in BLOCKED state.



