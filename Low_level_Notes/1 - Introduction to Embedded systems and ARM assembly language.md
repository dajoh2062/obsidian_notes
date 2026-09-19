
### Embedded systems

Textbook definition:
- Any device that includes a programable computer but not itself a general purpose computer.

Alternate definition:
- A computing system, specialized for only a few applications with none or minimum end user programmability, embedded into a larger product. The main purpose of the product is not computing.


![[{54F3867C-25AE-4115-A07D-6134C734E375}.png]]

![[{29D17053-7239-45D7-9F2A-61C84123C58A}.png]]

![[{3E38C7A5-11BB-4747-AEC0-276EB8EE6300}.png]]

Design objectives:
- Predictability: Essential to predict how a system is going to behave under any circumsstance before deployment.
- Dependability: CPS (Cyber-physical system) must operate dependably, safely, securely, efficiently and in real-time.
- Efficiency: Energy and run-time efficient. Weight and cost efficient. 


Properties:
- Reactive: reacting to stimuli from environment. 
- Time constrained: Right answers arriving too late are wrong.
- Specialized: Specialized towards a few applications or domains.



![[{F5B1DFD5-CF35-41E3-B456-9F8ADADF794F}.png]]
Embedded system design flow:
- Requirements -> Specification -> Architecture -> Components -> Integration.

### Designing Embedded Systems

Requirements:
- Functional: What should the system do. Eg. A GPS moving map should display the map of terrain around the users position.
- Non functional: Requirements not related to functionality such as size, power and so on. Eg. GPS moving map should fit on wrist, cost under x NOK per unit and last 8 hours.

Specification:
- Unambiguous technical description derived from requirements. Detailed enough to design system architecture. Examples: Fit on palm (4 x 6 cm), 8 hours battery (100mW).

Architecture:
- A high-level overview of system structure in term of components needed and their interaction.
![[{6C048173-519E-441F-AB50-7038D170B1CD}.png]]

Components:
- Choose or build the component to implement the architecture, and to meet the specifications.
- Standard components: CPU, memory, software libraries and so on.
- Custom components: Printed circuit boards, software modules, user interface.

Integration:
- Putting the components together and making the system work.
- Can cause unforeseen bugs. Careful component design helps.


![[{299C3254-597A-4081-95E8-484A99B5D451}.png|700]]


### Assembly Programming (ARM ISA)



Why?
- Processors native language.
- Can make efficient programs.

Instruciton Set architecture (ISA):
- Programmers view of hardware.
- Interface between hardware and software.
- Defines machine instructions, architecture state, memory managment.
- Abstracts away hardware implementation details.
- Divides software and hardware implemetation somewhat to increase compatibility.
- Enables multiple implementations (microarchitectures) of the same ISA.
- Specifics: 
	- What and how many instructions.
	- Number of registers.
	- Does the address need to be loaded to a register to access a memory.


ARM:
- Popular for mobile and embedded.
- Load-store architecture (register-register).
- Supports both little and big endianness.
- Variants:
	- ARMv8: 64-bit.
	- ARMv7: 32-bit.
	- Thumb architecture: 16 bit to reduce code size.
	- Thumb2 architeture: Both 16- and 32-bit instructions. We will use this one.

Assembly -> Machine instructions:
- Machine instructions are strings of binary numbers. Difficult for humans to interpret.
- Assembly instructions are a representation of machine instructions in a way thats easier for humans to understad. 
- Strict one-to-one mapping between machine and assembly.

![[{A751D484-17C9-4423-9D18-23299F3029DA}.png]]

Registers:
- Storage locations inside processor that hold variables and control state.
- Thumb2 provides 16 general-purpose registers and a current program status register (CPSR). Each is 32 bit.
![[{8FA4422B-13FD-45E9-B1B4-571E2186763F}.png]]

Thumb-2 instruction categories:
- Memory access instructions such as; ldr, str.
- Arithmetic/logic instructions such ass add, sub, shift, or, and. Only affect registers.
- Control transfer (branch) instructions; jumps, loops, compare and so on.

![[{BE041619-923E-448D-8CA1-15C684F3B08F}.png]]
Writing and assembly program:
![[{CD70D42F-1BB4-48CA-8942-7DA853272A8F}.png]]

