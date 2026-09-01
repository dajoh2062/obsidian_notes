
### Early history of parallel computing:

- 1758: Alexis-Claude Clairaut, Joseph Jerome Lalande and Reine Lepaute all sit together for 5 months and approximate the orbit of Halley’s comet.
- 1765: Neville Maskelyne begins calculating nautical almanacs for 1767 using himself and 5 employees.
- 1793: Gaspard de Prony initiates a 6-year effort to produce 19 volumes of trigonometry tables, using 96 untrained workers.

HPC and machinery:
- 1822: Charles Babbage announces Difference Engine #2 to the Royal Astronomical Society, its design is completed (but not yet implemented) in 1847.
- 1842: Augusta Ada King writes about the translation of calculations into machine states for its successor (the Analytic Engine), and inadvertently invents programming.

Early Computing Concepts:
- Maskelyne paid workers extra to finish calculations faster, similar to “overclocking.”
- De Prony created detailed instructions so untrained workers could perform complex calculations. Reusable instructions and tables were similar to modern library functions.
- Mechanical computers required fault tolerance because parts could fail.

Computers at War:
- Early high-performance computing was expensive and not very profitable.
- WWI created demand for calculations involving maps, ballistics, and navigation.
- Women performed much of this large-scale calculation work.
- WWII led to major investment in cryptanalysis at Bletchley Park.
- Alan Turing was among the researchers there.
- Konrad Zuse developed a programmable computer in 1941.
- IBM calculator pipelines were used for Manhattan Project calculations.

Computing After the Wars:
- Electronic and electromechanical computers developed rapidly after WWII.
- Reliability remained a major problem.
- John von Neumann studied how to build reliable systems from unreliable components.
- Robert Noyce integrated multiple switches onto silicon in 1959.
- Gordon Moore predicted continued transistor shrinking in 1965.

The Clock Race:
- Processor performance increased very rapidly from about 1959 to 2003.
- Higher clock frequencies were a major source of performance improvement.
- Around 2000, this growth began to flatten.
![[{7B8765EA-0F9C-46C9-ADFA-44F80765FB0E}.png]]

The Computing Boom:
- Post-war investment and semiconductor improvements caused enormous growth in computing power.
- Computers became important to the global economy.
- Computing changed from a specialized resource into a widely available commodity.
- This rate of improvement cannot continue forever.

Historical Lessons:
- Increased computing power can cause major changes in society.
- More computing power often requires long-term investment.
- Parallel computing was once the main way to increase performance.
- Today, parallelism has again become essential.

Why Parallel Computing:
- There is no real point where computing becomes “fast enough.”
- New computing power creates new applications that demand even more performance.
- Major problems such as climate modeling, medicine, and energy research require huge amounts of processing power.

Limits of Single Processors
- Increasing clock speed increases heat and power consumption.
- Processors cannot keep increasing frequency indefinitely.
- Very high clock speeds would make processors too hot.
- Modern systems increase performance by adding more cores instead.

Automatic Parallelization:
- Converting serial programs into efficient parallel programs automatically is difficult.
- A compiler may preserve correctness but still produce a slower program.
- Performance is harder to reason about than functional correctness.
- Efficient parallel implementations often require different algorithms.

The Future of Computing:
- Moore’s Law once helped overcome performance limits.
- Computers have become household products rather than huge investments.
- Scientific computing is no longer the main market driving hardware development.
- HPC increasingly depends on hardware developed for larger consumer markets.

The Wal-Mart Effect:
- Mass-produced hardware becomes much cheaper.
- Only 27 Cray-2 systems were built.
- Hundreds of millions of smartphones are sold.
- HPC benefits when its hardware is also useful in large consumer markets.

HPC and Consumer Technology:
- Hardware companies use HPC systems as test environments for new technologies.
- Technologies developed for supercomputers often later appear in consumer devices.
- Vector processing moved from supercomputers into normal CPUs.
- Large parallel systems eventually became compact accelerator cards and multicore desktops.

What the Course Covers:

- The course uses small physics simulations as examples.
- The focus is on parallel programming models.
- MPI is used for message passing.
- pthreads are used for thread-based programming.
- OpenMP is used for shared-memory parallelism.
- CUDA is used for GPU programming.

Why Learn Parallel Computing?

- Modern supercomputers already rely heavily on parallelism.
- HPC techniques often become relevant to consumer computers later.
- Learning parallel programming prepares you for future hardware and software challenges.

Main Takeaway:
- Hardware became strongly parallel around the early 2000s.
- Modern systems continue adding more processors, cores, and accelerators.
- Software has not adapted as quickly as hardware.
- Efficiently using multiple processors is therefore an important programming skill.