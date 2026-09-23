
Instruction fetch:
- Want to fetch as many instructions per cycle as possible.
- Branch prediction tries to fill the pipeline with correct-path instructions.
- Importance increases with deeper pipelines.

Branch prediction goal:
- Predict direction and target address, start fetching and executing.
- Branches exhibit temporal locality. 
- Keep track of past history, and try to predict.
- Typically over 90% (90-99%) of dynamically executed branches are correct.


Static branch prediction:
- A single static policy for all branches is unlikely to work.

Branch history table (BHT):
- Hash the PC of a branch into small buffer which stores the branch outcome and use for making prediction.
- 2 bit predictor does not change its prediction when only wrong once.
- Pretty good score (80-99%), mean 90%.

![[{2C719654-0CC6-4BDE-B590-9464784ADC94}.png]]
Cost per mispredicted branch:
- Proportional to pipeline depth/stages.
- Early pipelines had 4-6 stages, now 12-15 (higher clock frequencies).


![[{E121ACBF-EC2A-45E9-9ECF-A7AB0086C2A7}.png]]![[{49388A75-E2D2-4A8B-A957-80902416BCC2}.png]]



### Static branch prediction

Static prediction:
- Before program execution.
- One prediction per static branch in the program binary.
- Via software, either compiler or programmer.

PRO:
- Easy to implement.
- Little needed HW.

CON:
- Provides same prediction regardsless of input or history.

Three flavors:
- Rule based.
- Program based.
- Profile based.

Rule based:
- Always not taken: Sequential fetch.
- Always taken: HW more complex becuase unknown branch target. May lead to lost cycle in pipeline.
- BTFNT: Backwards addresses taken, forward not taken.

Program based:
- Requires hint in instruction opcode.
- Estimated based on program structure.
- Examples: Predict loop branches to be taken, non-null-pointer path, non equal pointer path.
- Usually better than rulebased.

Profile based:
- Do a test-execution with training input to collect information and branch counts.
- Use profile during recompilation with hint-bits.
- Predict if >50%.
- Typically more accurate than program/rule.
![[{46D28C07-4BF7-4A80-BC3C-472DE112D7B3}.png|586]]
### Dynamic branch prediction

Dynamic prediction:
- During program execution.
- Muliple predictions per static branch, depending on history.
- Done in hardware.
- More accurate than static (80-97% vs. 50-80%).
- Some branching is hard to predict statically but easier dynamically: alternating taken/not taken, taken during second half.
- Takes into account branch context.

Bimodal predictor:
- Uses history of branch address only.
![[{C2B1F314-84A8-4202-AA96-FB6D99839CA2}.png|565]]
![[{8CFBCCF2-8975-4630-9617-450C93B9CED6}.png|559]]

![[{544BA485-4105-4445-B6A8-ECDFEB7D7A4F}.png]]
Two-level predictors:
- Uses history of branch address but also:
	- local history of particular branch
	- global history of all branches
- works because some branche conditions depend on the same variable.
- branching can be correlated because of a connected variable.
![[{C1CBCCA6-62F7-4050-BA20-6B87392E4499}.png]]![[{7E20FD72-E833-4F7C-8776-A9ED5464C5BB}.png]]Two-level implementations:
- If m=0:
	- Global pattern history table, all branches share the same pht (gPHT).
- If m != 0:
	- per address pattern history table (pPHT).
	- Pht is partitioned based on branch address bits.
- Four variations:
	- GAg, GAp, PAg, PAp.
	- G global history, p per address.
	- A adaptive.
	- g = gPHT, p = pPHT.


Good configurations:
- GAg: BHR: 18 bits, PHT: 2^18 x 2 bits.
- PAg: BHT: 2^11 x 12 bits, PHT: 2^12 x 2 bits.
- PAp: BHT: 2^11 x bits, PHT: 2^9 x 2^6 x 2 bits.
- Prediction accuracy of 97%.

GAp vs. gshare
- GAp concatenates history and address bits, requiring a choice of how many bits to use from each.
- gshare XORs history and address bits, using more information for the same PHT index size.

Why mispredictions happen
- Difficult branch behaviour or insufficient predictor training.
- Aliasing: branches with different behaviour share and interfere with the same PHT entry.
- Predictor type does not match the branch’s behaviour.

Hybrid and tournament predictors
- Combine predictors, typically local and global history, and learn which to trust.
- A 2-bit meta predictor selects P1 for values 0–1 and P2 for 2–3.
- Only P1 correct: decrement. Only P2 correct: increment.
- Both correct or both wrong: no meta-predictor update.
- Always update both component predictors, regardless of which was selected.

Processor examples
- Alpha 21264 combines PAg and GAg.
- PAg: 1K × 10-bit local histories; 1K × 3-bit prediction counters.
- GAg: 12-bit global history; 4K × 2-bit counters.
- Alpha’s meta predictor: 4K × 2-bit counters, indexed by global history.
- IBM POWER4 combines bimodal and gshare; each predictor and the meta predictor has 16K × 1-bit counters.
- POWER4 uses 11-bit global history and gshare indexing for the meta predictor.

Branch Target Buffer (BTB)
- Predicts the target address; direction prediction determines taken/not-taken.
- Small, typically set-associative cache mapping branch addresses to target addresses.
- Accessed alongside the instruction cache to supply the next cycle’s fetch address.
- Handles conditional and unconditional branches; also called BTAC.

Return Address Stack (RAS)
- Returns need special handling because a function can have different callers.
- Call: push the return address, i.e., the instruction after the call.
- Return: pop the address and use it as the predicted target.
- Limited capacity can cause mispredictions with deep nesting; Pentium 4 has 16 entries.

Prediction in the pipeline
- Illustrated design: fetch uses BTB/RAS; decode predicts direction and calculates PC-relative targets.
- Correct target: fetching continues without interruption.
- Wrong target: discard the incorrect instruction and redirect fetching, creating a one-cycle bubble in the example.

Speculation and misprediction recovery
- Fetch and execute along the predicted path, but do not commit speculative results.
- Multiple unresolved branches can coexist; tags track speculation dependencies.
- Branch outcomes become known during execution.
- Correct prediction: release its tag; instructions may still depend on other unresolved branches.
- Misprediction: squash wrong-path instructions and fetch from the correct path.

Key takeaways
- Prediction keeps the pipeline filled with useful instructions; deeper pipelines make accuracy more important.
- Typical accuracy given in the lecture: 90–99%.
- Static direction prediction: rule-, program-, and profile-based.
- Dynamic direction prediction: bimodal, two-level, and tournament.
- Target prediction: BTB and RAS.