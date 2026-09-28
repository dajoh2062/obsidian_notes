
Objectives:
- Numerically integrating various functions accounts for a lot of applications in parallel computing.

Advection and diffusion:
- Diffusion: tendency to level out.
- Advection: How to medium it is in carries it along when moving. 
- Spreading out from dense concentration to less concentrated. Eg.: Heat, gases, water pollution.
- Example: Heating apartment without a fan vs. with a fan next to heating source.

Advection terms:
- U: Amount of whatever is spreading out.
- t: time axis.
- x: space axis.
- du/dt: difference in u when time changes.
- du/dx: difference in u from left/right in space.
- v: how quickly the medium is moving, and therefore transporting some of the U in either direction. 

Equation:
- Integrating would give us a function U(t, x).
- du/dt tells us if we will find more U at this given point later/earlier in time, and du/dx tells us how the concentration moves through space.
![[Screenshot 2026-09-28 at 13.59.48.png|368]]
Taylor polynomial to solve f'(x) from f(x) and dx:
![[Screenshot 2026-09-28 at 14.27.08.png|604]]


![[Screenshot 2026-09-28 at 14.26.23.png]]
![[Screenshot 2026-09-28 at 14.27.36.png]]

![[Screenshot 2026-09-28 at 14.29.57.png]]

Expression for change in U in time:
![[Screenshot 2026-09-28 at 14.36.47.png|577]]
Advection equation looking forward in time, and left and right in space:
![[Screenshot 2026-09-28 at 14.57.46.png]]

Boundary conditions:
	- Dirichlet: Outside points equal a constant.
	- Neumann: Mirror inside points.
	- Periodic: Outside points connect.


Small problem:
- Making small rounding errors for every time step.
- Replace one term on the right side adds some friction instead.
- New formula:

![[Screenshot 2026-09-28 at 16.44.57.png]]
Main stages of a simulation program:
- Initialize.
- Integrate with loop.
- Finalize.
