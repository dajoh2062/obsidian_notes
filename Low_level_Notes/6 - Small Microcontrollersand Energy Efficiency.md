
Microchip:
- Microchip Technology is an American company that designs and manufactures semiconductors.
-  Its products include microcontrollers (small computers that control functions inside devices), chips for power management and connectivity, memory, and security.

### Basic electronics

- Voltage  = Resistance \* current.
- Power = Voltage \* current
- Power Consumption \[W]  = \[J] / \[S]
- Watt Hours = \[W] \* \[Hr]
- Ampere hours = \[A]\*\[Hr]

![[{0307895D-010F-4604-8E4B-94755187F617}.png|700]]


### Microcontroller architecture

![[{EFD2CBAD-5AB2-4EF5-8AA3-379BFFED9A3D}.png]]
AVR Architecture:

![[{6E85CB18-E3F9-4FF9-836E-0212485D2788}.png]]

PIC Architecture:
![[{EF5B55B4-8530-46F5-824D-B5D4AAE83DF7}.png]]
The main differences are:
- CPU design: AVR and PIC use different basic instructions and ways of doing calculations.
- Registers—tiny storage spots inside the CPU: AVR usually has 32 working registers; common 8-bit PICs do many calculations through one main working register, called W.
- Programming: The program must be built for the chosen chip. An AVR program cannot simply be loaded onto a PIC.
- Features: Memory, speed, pins, and built-in tools depend on the specific model, not just whether it’s AVR or PIC.

Microcontroller typical applications:
- Low power applications: Battery powered devices, IoT applications.
- Distributed logic and coexistence with larger processors: real-time control tasks, reading sensors, controlling simple user interfaces.

Microcontrollers concrete examples:
- Smartwatch, kitchen applicances, digital gadgets, vechicles.


### Low power:

![[{008E7344-8E4A-4453-89E3-CA55D502437C}.png]]
![[{16D197BC-4C47-46EE-B879-BE4FA1893B45}.png]]

Battery capacity:
- Battery time \[Hrs]  = Battery capacity \[mAh] / Current consumption \[A]


### Low power techniques

1 - Operating modes (AVR example):
- Active mode: All clocks active, CPU is running, Power consumption is proportional to the frequency of the system clock.
- Idle mode: CPU stops executing code, no peripherals are disabled, all interrupt sources can wake the device.
- Standby mode: Peripherals can be configured to be enabled or not, power consumption depends on what functionalities are enabled.
- Power-down: Only few peripherals are active, fewer wake up sources.
- Total power consumed = Active mode power + standby/sleep mode power +wake up power.

![[{488038AF-1DD1-4838-A58F-CF8699BBEFA4}.png|479]]
![[{BB2B5FEA-D1C8-48A8-8773-C83C619537F1}.png]]
2 - Busy-wait vs. Polling:
- Busy- wait: Checking the condition continously, keeping the CPU fully enganged.
- Pro: Low latency. 
- Con: High power consumption.
- Use case: Short, immediate waits.
![[{C8927A9F-C870-4D2A-ADBE-13D6A10A869D}.png|347]]
- Polling: Periodically checks a condition at certain intervals, allowing CPU to multitask or enter low power mode.
- Pro: Moderate power consumption.
- Con: Higher latency.
- Use cases: Less critical, periodic checks.
![[{C84490B7-592A-4510-8F3C-EE2219002F91}.png|461]]
- Interrupt-driven approach: Uses hardware interupt to notify cpu of an event, freeing the CPU to do other tasks or low power mode.
- Pros: Very low latency, low power consumption.
- Use cases: Real-time and power sensitive.
![[{0D29C0ED-A343-4395-9185-4A6B8FCA786A}.png|608]]
3 - Core independent Peripherals (CIPs):
- Peripherals capable of handling tasks without intervention from CPU. 
- Frees CPU time. 
- Provide short and predictable response times between peripherals. Reduces complexity and execution time
- Examples: Event system, Configurable custom logic, timers, counters, Analog to digital coverter.

4 - Event system:
- Peripheral to peripheral signaling.
- Enabled through event channels, not using the CPU.
- Reduces complexity, size and execution time of the software.
![[{1E11D7ED-C8D4-4AE9-86D5-EBC24D1C8028}.png]]

5 - Choice of Oscillator:
- Choice of oscillator and clock speed impacts power consumption.
- Lower frequency = lower power. usually.
- Tradeoff between accuracy and power consumption.
- Sometimes a higher frequency is advantageous, returning to lower power modes quicker.

6 - Unused Pins:
- Unused pins are chip connections you aren’t using.
- If left floating, their voltage is not held at a fixed level.
- Electrical noise can make them read randomly as 0 or 1, wasting power.
- An internal pull-up resistor holds the pin at a stable high voltage.
- Disabling the input buffer turns off the circuit that reads the pin, saving more power.
- The code applies both settings to all eight pin positions in a port—use this only if those pins are unused.
![[{33106493-A900-4391-AFEE-3910906F591B}.png]]


### Programming microcontrollers:


Languages:
- C, Embedded C, C++, python, java and more.

IDE:
- Arduino IDE, MPLAB X (VS extension).
- 1: Create project with main.c file.
- 2: Compile and build project.
- 3: Upload code (hex file) to MCU.

Machine language:
- Binary 0 and 1s. 

![[{A06433C7-1051-4803-9B29-FDFF8802BA57}.png]]![[{0963E8DF-B706-44F8-BAAA-10BD3A25CD63}.png]]

Bitmasks and bitwise operations:
- AND: &=
- OR: |=
- XOR: ^=
- NOT: ~(bits)


Analog comparator (AC):
- Output 1 when difference in volage is positive and 0 otherwise. 

![[{DCD865F8-58F5-4C71-9D1D-7CA98CE01817}.png|503]]
