Energy efficient computing, motivation, theory, technology trends and accellerators.

# Pre lecture slides

Power vs energy:
- Power = Energy/time
- \[Watt] = \[Joule]/\[Second]
- Servers and desktop computers have the thermal envelope as their limit as overheating is the main concern (power), while phones and portable devices have the battery size as the limiting factor (total energy).


Dynamic power:
- Power consumed when a circuit switches between 0 and 1, usually when doing a useful activity.![[{F7AA6500-C880-4F80-9E48-F5839C722B23}.png|295]]
Static power:
- Power consumed by circuit while doing nothing. Transistors leak even in offs state.![[{BF140430-26B2-4E51-87E1-71E75BF948B6}.png|352]]
Reducing energy consumption:
- The smaller the transistors, leakage powers portion increases.
- Hardware techniques to decrease: Clock gating, power gating.
- Software techniques: Sleep modes, dynamic voltage and frequency scaling (DVFS).


Clock gating:
- Turning of clock signal to circuit that is currently unused.
- Keeps store value as long as EN = 0, which sets clock at 0. Makes frequency (f) 0.

Power gating:
- Turns off power to the circuits not in use.
- An ordinary register loses its stored value when its supply is disconnected. After power returns, its value must be initialized or restored.
- Leakage is reduced way more than clock gating.
- Energy Overhead: require some energy to power down and power up a unit, so the energy saved should out-weigh.
- Performance overhead: Extra time required in relation to activating the unit.

OS power managment:
- Hardware provides multiple power modes, which Os can use to reduce consumption.
- Sleep modes provide maximum power saving. Similar to power gating but with software. Overhead on entering and exiting. Multiple levels to adapt the tradeoff.


Dynamic voltage and frequency scaling (DVFS):
- Lets OS choose between multiple voltages and clock frequencies.
- Reducing frequency reduces dynamic power consumption, but energy remains the same.
- Reducing voltage reduces both dymaic and static power consumption.
- OS needs to adjust voltage and frequecy such that the CPU runs just fast enough for task.

# Lecture slides

### Bare metal, runtime systems, and operating systems

Bare metal programming:
- Software runs directly on hardware. No system calls, libraries or drivers.
- Start from instruction at address 0.
- Reset handler must set up the stack pointer and call main.
- Minimal portability, no hardware abstraction and harder debugging.
- Max performance and control. Predictable timing, ideal for Realtime systems.

Runtime environment (RTE) and runtime system (RTS):
- Memory management (stack, heap/allocated memory, garbage collection).
- Aspects of programming languages that needs to be handled during run time, such as communcation between processes/threads, signals, dynamic loading and more.
- Distinction between runtim system and operating system is not clear.
- Examples:
	- FreeRTOS (from 1-2 kB RAM, from 5 – 10 kB ROM (e.g. FLASH))
	- Zephyr RTOS (from, 6-8 kB RAM, from 3-8 kB ROM)

Operating system (Os) and Real time Operating system (RTOS):
- File system, process management, security, networking and more.

![[{1FC1BD7E-8DEB-4E0A-9A13-6B107FB8BD99}.png]]
![[{EE06A4DE-5D99-4BD3-89DD-BB87925E91AA}.png]]![[{F69E6FC6-A8CC-45B4-A7FF-1269B9A543D0}.png]]
![[{4CC57407-AD57-4F28-AC38-F9396A9A23A1}.png]]
### Measuring performance

Performance monitoring unit (PMU):
- Counts hardware events that help explain execution time.
- Such as clock cycles, instructions completed, cache references, cache miss, branch mispredicts.
- Derived metrics: , instructions per completed clock cycle, cache miss rate, branch misses per instruction executed.

![[{FADC75DB-6457-4C50-83EF-66C89D2D8FD1}.png]]
For reliable measurements:
- Repeat experiments to reduce uncertainty.
- Distinguish cold-cache execution from execution after warm-up.
- Control interference from other tasks.
- Consider strided access: stepping through memory can use cache lines less efficiently than accessing adjacent elements.

Why use PMU:
- Big-O describes growth with input size; it does not capture instruction counts, memory accesses, or pipeline behavior. The counters show differences that execution time alone cannot explain.


### Sustainable computing
![[{462FEA5E-78CE-44DC-B552-C9001B11D0E0}.png]]
Sustainalbe computing:
- Reducing the environmental impact of computing itself.

Computing for sustainability:
- Using computers to improve other systems, such as transport or energy supply.

Operational footprint:
- Impact from using equipment, including electricity and cooling.

Embodied footprint:
- Impact associated with manufacturing and other lifecycle stages beyond operation.
- A computer’s environmental cost includes raw materials, production, transport, use, maintenance, and disposal.
- Examples of reducing embodied impact include extending hardware lifetime, producing fewer chips, and designing smaller chips.

Jevons paradox: 
- Improving efficiency can make a resource cheaper to use, increasing demand enough that total resource consumption rises. Better efficiency per computation does not guarantee lower overall consumption.
![[{16C343C3-08A3-42C0-BFCB-29FC8E741431}.png|593]]
### Technology trends and the power wall

Moore’s law (slowing/stopped?):
 - Historical trend of increasing transistor counts per chip.

Dennard scaling (has stopped):
- Historical ability to shrink transistors while keeping power density roughly constant.
- Under ideal Dennard scaling, halving transistor dimensions quarters their area and power. More transistors could therefore fit into the same area without increasing power density.
- That scaling broke down because voltage and current could no longer decrease sufficiently while maintaining reliable operation. Leakage also became a larger problem.

Power wall:
- Power and cooling limits restrict further performance increases.

Dark silicon (not sustainable):
- Parts of a chip cannot operate simultaneously within its power or thermal budget.

Parallelisation, vectorization and specialization:
- Requires more complex programming but can improve energy efficiency.

![[{E4915225-947B-4350-8D3B-BE20479A0CFD}.png]]
### Power, energy, and gating

Dynamic power:
- P_dynamic =  a \* f \* C \*  V\^2
- Where a is activity factor and C is how much capacity we have.
- Energy used caused by transistor switching.

Clock gating:
- Turning off clock to circuits not in use, reducing P_Dynamic by reducing activity factor temporarly.

Static power:
- P_static = I_leakage \* V
- \[W] = \[A]\*\[V] 
- Increases with temperature, and with smaller transistors.
- Leakage current, even when not used.

Power Gating:
- Turning of power supply to circuits not in use, mainly reducing P_static by decreasing voltage. 
- More overhead than clock gating.

![[Screenshot 2026-10-26 at 14.43.00.png]]
![[Screenshot 2026-10-26 at 14.46.18.png]]


### Thermal limits and measurement

Dennard scaling slow down and end (estimated 2005-2010):
- Transistor size decreased while power denisty was stable, allowing more transistors.
- Stopped due to not possible to drop voltage and current levels in proportion.
- Heating and static power leakage become a larger problem at smaller transistor sizes.
- Makes parallelism and multi core more important, in processors and memory, due to single core performance tapering.

Dark silicon problem:
- Because of power and thermal limitations, parts of a chip must remain powered off.
- This makes processors with smaller sized transistors, and same total size and power not gain the proportional performance.
- Also for parallel computing: Even if manufacturers can fit more CPU cores onto a chip, the power budget may prevent all of them from running simultaneously.

Thermal constraints:
- When a processor consumes electrical power, most of that energy eventually becomes heat.
- If the processor generates too much heat, it can overheat.
- Usually the constraint for human touched devices.

Power constraints:
- A power budget is the maximum electrical power a device or system is allowed to consume.
- A data center has limitations on how much electricity its infrastructure can deliver.
![[Screenshot 2026-10-26 at 15.05.40.png]]

Difference electrical enery and Power:
- Electrical energy: Amount of energy transferred to other forms (heat) in an electrical circuit during some amount of time. Measured in Joules ( J = W \* S)
- Power: Work done” (electrical energy transferred) per unit time. Measured in Watt (W = J / s). Also: P (watt) = U (Volt) \* I (Ampere).

Example 1: Raspberry Pi4:
- 3-6 watt.
- Runs at 5.1 volts, current depends on peripherals.
- With 6000 mAh (three AA batteries in series), it lasts only 3 h 20 min in idle.
![[Screenshot 2026-10-26 at 15.44.03.png|462]]

Example 2: AVR128DB48 Curiosity Nano:
- 13μ Watt – 39m Watt.
- Runs at 1.7-5.1 volts.
- With a 240mAh battery it lasts 20 years in idle!
![[Screenshot 2026-10-26 at 15.44.32.png|593]]

Thermal design power (TDP):
- Represents the average power the processor dissipates when operating at base frequency with all cores active under a high-complexity workload.

Hyperthreading:
- Allows one physical core to appear as two logical cores, to the OS, and therefore handle double the amount of threads. This makes it possible to better multiplexing multiple lightweight threads or processes.
- Seperate registers for each hyperthreaded core is what gives the performance boost, in addition to OS waiting times.
- HT is Intels marketing name for Simultaneous Multithreading (SMT).

Turbo boost:
- Dynamically increase clock to enhance performance when needed (within limits of TDP).
- Dynamic overclocking.

Cooling solutions:
- Heat sink: component that absorbs heat from a CPU or other electronic component and transfers it to the surrounding air to prevent overheating.
- Thermal interface materials (TIM): Thermal paste placed between the chip and the heat sink to improve thermal conductivity and ensure efficient heat transfer.
- Active cooling with fan.
- Water cooling.

Relevance to low-level programming:
- Reading temperature, fan speeds, controlling fan speeds.
- Controlled temperature is necessary for precise energy measurements.
- Temperature affects on-chip energy measurements.
- Made possible by Intel RAPL (Running Average Power Limit)

![[Screenshot 2026-10-26 at 16.58.47.png|651]]



### The vertical approach and efficiency metrics
![[Screenshot 2026-10-26 at 17.00.06.png]]
Computer architecture:
- the way a computer’s hardware components interact with each other and with the software.

Vertical Approach to Energy Efficiency:
- Main idea: Improve energy efficiency across all levels of a computer system, from software to hardware.
- Applications: Optimize algorithms, data structures, programming languages and parallelization.
- System software: Optimize operating systems, compilers, task scheduling, resource management and DVFS.
- Computer architecture: Improve multicore systems, accelerators, caches, memory and communication.
- Holistic approach: Consider how hardware and software interact rather than optimizing each layer independently.

Energy Efficiency Metrics:
- Energy: \(E=P\cdot t\), measured in Joules (J).
- Optimization goals: Maximize performance, minimize energy consumption, or balance both.
- Performance per Watt: FLOPS/Watt, used by Green500 to rank energy-efficient supercomputers.
- Energy Delay Product (EDP): \(E\cdot t\), measured in Joule-seconds (Js). Lower is better.
- Energy Delay Delay (EDD): \(E\cdot t^2\), measured in \(Js^2\). Penalizes slow execution more heavily.
- Benchmarks: SPEC evaluates CPU performance; EEMBC focuses on embedded processors.
![[Screenshot 2026-10-26 at 17.09.44.png|453]]

### Parallelism and specialization


Vectorization (SIMD):
- Vectorization: Performs the same operation on multiple data elements simultaneously (SIMD = Single Instruction, Multiple Data).
- History: Popular in Cray supercomputers (1976–1990), later returned in mainstream CPUs.
- Intel SIMD: MMX (64-bit), SSE (128-bit), AVX (256-bit), AVX-512 (512-bit).
- ARM NEON: SIMD extension used for audio, video, graphics and gaming.

Energy-Efficient Computing:
- Hardware techniques: Low-power design, DVFS, sleep modes, multicore and specialized hardware.
- Software techniques: Efficient algorithms, data structures, compilers and operating systems.
- End of Dennard scaling: Forced architects to exploit parallelism for better energy efficiency.
- Architecture-level optimization: Offers greater potential power savings than low-level circuit optimizations.

 Chip Multiprocessors (CMP):
- CMP: Multiple processor cores on the same chip, potentially sharing cache.
- Throughput: Multiple slower cores can achieve similar throughput to one fast core for parallel workloads.
- Energy efficiency: Lower frequency allows lower voltage, significantly reducing dynamic power.
- Example: Under ideal assumptions, 2 cores at half frequency and voltage consume only 25% of the original dynamic power.
- Limitation: Parallelization introduces overhead, and not all applications can be parallelized efficiently.

Multicore and MPSoC:
- Multicore: Modern name for Chip Multiprocessors (CMP).
- Cost: Several smaller processors can be cheaper than one very powerful processor.
- Real-time systems: Dedicated cores make timing guarantees easier.
- SoC (System on Chip): Integrates multiple components onto one chip, reducing cost.
- MPSoC: SoC containing multiple processors; physically separated processors form a distributed system.
![[Screenshot 2026-10-26 at 18.34.29.png]]
Accelerators and Parallelism:
- Accelerators: Specialized hardware for specific workloads, such as GPUs, DSPs and NPUs.
- BLP: Bit-Level Parallelism.
- ILP: Instruction-Level Parallelism.
- TLP: Thread-Level Parallelism.
- DLP: Data-Level Parallelism (e.g., SIMD).
- ALP: Accelerator-Level Parallelism — multiple accelerators execute different workload components concurrently.
- Example: Samsung Exynos 9810 combines CPU cores and an ARM Mali GPU in one SoC.

![[Screenshot 2026-10-26 at 18.31.02.png]]