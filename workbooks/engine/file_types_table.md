# Engine file types

Every file extension the engine and its projects write or read. A new extension is Doug's to name. Before one is
proposed, it is checked against this table and against the whole biohub tree, anchor_sift_python included.

## In use

| Extension | What it is | Named by |
|---|---|---|
| `.kcr` | The information crystal (Kolmogorov crystal). krep kind "KCR\0", version 1. The only crystal format. | Doug, 23 September |
| `.krs` | A ruleset: how the emitter spells the forms in one language (`ptx.krs`, `c.krs`, `vhdl.krs`). | |
| `.kcs` | A construction set, part of the crystal flattener. | Doug |
| `.knf` | A sample's noise floor. | |
| `.ksh` | The flattened set: every body of every sample as one number each (`train.ksh`). | |
| `.ans` | The answer file. | |

## Retired

| Extension | What it was |
|---|---|
| `.iapx` | The crystal before `.kcr`. Not read; sets are re-ingested. |

## Offered, not adopted

| Extension | Offered for |
|---|---|
| `.kfc` `.kst` `.kgr` `.ksd` | The other apx files (flattened, OAPX history, BAPX bodies, IMP key); still open with Doug (build_plan.md). |
| `.khw` | The hardware constraints file the VHDL state-boundary pass reads (clock budget per state, cost of each form, memory ports and read latency, the forms that end a state). Awaiting Doug. |
