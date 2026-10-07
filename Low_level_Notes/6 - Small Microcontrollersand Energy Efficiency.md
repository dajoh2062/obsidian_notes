
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


### Low power modes


