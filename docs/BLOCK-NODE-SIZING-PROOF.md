# Block Node Sizing Proof

Pluruno has completed a controlled Block Node sizing experiment for the current distributed MoE proof model.

The purpose was to test whether moving multiple sequential transformer layers behind one remote Block Node boundary can reduce external transport work without changing model semantics.

## Tested scenarios

Four trailing-block sizes were exercised and accepted:

- 1 layer
- 2 layers
- 4 layers
- 8 layers

All four scenarios completed successfully in the controlled revalidation.

## Observed results

Relative generation-time change versus the comparison baseline:

- 1 layer: **+6.73%**
- 2 layers: **+14.67%**
- 4 layers: **+9.25%**
- 8 layers: **-6.73%**

The measured external physical traffic remained close to **1.0 MB** across the four scenarios, while the amount of expert work executed inside the Block Node increased with block size:

- 1 layer: ~1.01 MB external / ~5.51 MB internal expert traffic
- 2 layers: ~1.02 MB external / ~11.01 MB internal expert traffic
- 4 layers: ~1.02 MB external / ~22.02 MB internal expert traffic
- 8 layers: ~1.02 MB external / ~44.04 MB internal expert traffic

## What this proves

This experiment does **not** establish that larger blocks are always faster.

It proves a narrower and more useful point: Pluruno can execute several sequential transformer layers inside one remote deployment unit while keeping the external transport boundary roughly constant. In this specific controlled run, the 8-layer case was also the only tested size that improved generation time relative to the comparison baseline.

That supports Block Nodes as a topology option for reducing repeated network crossings when peer capability and locality make multi-layer execution advantageous.

## Architectural implication

Pluruno should not force every heterogeneous peer into the same deployment shape. Depending on measured capability and network conditions, a peer may be more useful as an Expert Node, Expert Group, Session Executor, or Block Node.

Block sizing therefore remains a measured placement decision rather than a fixed global constant.

## Scope

These results are from a controlled lab proof. They do not yet claim production Internet performance, universal optimal block size, or performance across every model architecture.
