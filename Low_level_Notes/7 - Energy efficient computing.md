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



### Thermal limits and measurement

### The vertical approach and efficiency metrics

### Parallelism and specialization
