Operating systems:
- Dekstop: Windows, linux, macos.
- Mobile: android, ios.
- Embedded os: Blackberry QNX, GreenHills software, arm mbed os.

Why are desktop os suitable for embedded devices:
- Usually monolithic or hybrid, providing numerous services for diverse hardware.
- Too large memory usage and disc space requirements.
- Dont provide special features needed: Real time guarantee, reliability by certain standards.

DO-178C:
- Software Considerations in Airborne Systems and Equipment Certification.
- Applications divided into 5 levels:
	- Catastrophic: Failure may cause deaths, and loss of airplane.
	- Hazardous: Failure has a large negative impact on safety, performance, or reduces the crews ability to operate the aircraft safely, and may cause fatal injuries among passengers.
	- Major: Failure significantly reduces safety margin or increases work load, may cause injuries.
	- Minor: Failure slightly reduces safety margin, or increases work load, such as rerouting or other inconveniences.
	- No effect: Failure has no impact on safety, aircraft operation, or crew workload.
![[Screenshot 2026-10-27 at 14.26.46.png]]

Embedded os:
- Diverse and manage various types of software.
- Many are designed with only one or a few specific functionalities.
- Usually resource constrained.

Embedded Operating systems Goals:
- Customizable: Functionalities.
- Resource efficient: small memory footprint.
- Predictable and deterministic: real-time capabilities.
- Energy efficient: dynamic power management.

Processes and threads:
- Two objects to be scheduled by OS.
- Different states during execution:
	- READY: Ready to run and pending.
	- RUNNING: Being executed by OS.
	- BLOCKED: Suspended due to an event, such as I/O operation.
- Scheduling: Policies that OS uses to determine execution order of ready objects.
- Scheduling algorithms have diverse objectives and demonstrate different effects on performance. Includes round-robin, completely fair scheduler, multi-level feedback queue and more.
![[Screenshot 2026-10-27 at 14.54.23.png]]
Completely Fair Scheduling (CFS):
- Default CPU scheduler in Linux.
- Choose the process with lowest virtual execution time: vruntime.
- Balanced binary search.
- Complexity: insertion, deletion and update is O(logN) and find minimum is O(1).
- Goal is fairness, and not real-time deadlines.


Real time guarantee/requirements:
- Many embedded systems have requirements for not only correctness of computation, but also time at which the results are produced.
- Predictability and determinism.
- Classification of consequence of missing deadline:
	- Hard Real-time: Catastrophic consequences, safety critical applications.
	- Soft Real-time: desirable features such as information systems or video.

RT extension for OSs:
- For linux: Used in som safety critical systems such as NASA. 
- Some time-sensitive high-performance computing applications emerged.
- PREEMPT_RT for linux: Makes more of the kernel preemptible, allowing high-priority tasks to run sooner.
- SCHED_DEADLINE: Three parameters; runtime, period and deadline. Earliest deadline first principles.
- Still cannot achieve the same level of real-time performance as real-time operating systems.


### Real-time Operating Systems (RTOS)

What:
- OS that supports real-time scheduling and guarantee real-time deadlines.
- Typically microkernel based.
- Examples: Blackberry QNX, Zephyr, arm mbed os, Greenhills, freeRTOS, threadX.

![[Screenshot 2026-10-27 at 15.37.51.png]]

Real-time scheduling:
- Algorithms used to make tasks meet deadlines.
- Know more about the tasks.
- Such as: Deadlines, periods, worst-case execution time (WCET).
- Many embedded systems are closed systems.
- Static scheduling algorithms: Scheduling based on fixed parameters before execution. FPS, RMS.
- Dynamic scheduling algorithms: Scheduling based on dynamic parameters that may change at runtime. EDF, Least slack time.

Fixed priority scheduling (FPS): 
- Each task is assigned a priority number that doesnt change during execution, and tells CPU which task to run with the highest priority.


Rate-monotonic Scheduling (RMS): 
- Specific type of FPS. The task with the shortest period gets highest priority. Priority is frequency of task. 
- Simple to implement, widely used.
- Assume deadline = Period.
- No resource sharing and no context switching overhead.
- CONS: Lower system utilization.

![[Screenshot 2026-10-27 at 16.10.47.png]]

Earliest Deadline First (EDF):
- Closest absolute deadline gets the highest priority.
- Dynamic priority.
- Preemptive.
- Cons: Less common, complicated to implement.
- Assumptions: Deadline = period, uniprocessors, no context switching overhead, no resource sharing.
- Schedulability test: 
![[Screenshot 2026-10-27 at 16.32.17.png|476]]


![[Screenshot 2026-10-27 at 15.55.30.png|677]]
![[Screenshot 2026-10-27 at 15.59.43.png]]
Priority inversion:
- Accessing critical sections should be atomic.
- Guarantee mutual exclusion using mutex lock or semaphore.
- Priority inversion is when a high priority task is indirectly blokced by a low priority task, due to the lower priority task has obtained a variable lock the higer priority needs.

![[Screenshot 2026-10-27 at 16.44.44.png]]
Priority inheritance:
- A solution to priority inversion problem.
- when a high-priority task has to wait for some resource shared with a running low-priority task, the priority of the low-priority task is temporarily promoted to the highest priority level of the pending tasks.
![[Screenshot 2026-10-27 at 16.45.55.png|456]]

Energy efficient scheduling:
- A lot of embedded systems are battery powered, and energy consumption is one of the main design concerns.
- Linux kernel; energy aware scheduling.
- Energy consumption: E = P \* T
- Power consumpton: P = P_dynamic + P_static
- Dynamic voltage and frequency scaling (DVFS): Ised to slow down processor by scaling down frequency and voltage. Use voltage and frequency just enouch for tasks to meet deadlines. ![[Screenshot 2026-10-27 at 16.52.01.png|482]]
- Turn of cpu to reduce static power consumption, or enter lower power modes. 

Governors:
- A CPU governor controls the processors frequency and voltage to balance performance and power consumption.
- Governors on linux:
	- Performance: Runs CPU at max frequency.
	- powersave: runs CPU at min frequency.
	- ondemand: adjusts CPU frequency based on workload.
	- schedutil: Integrates with the scheduler for better responsiveness.
- Ondemand was popular, and still used on Rpi os and other embedded OS.
- Schedutil has become default CPU governor in many OS.



