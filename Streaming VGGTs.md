**StreamVGGT**
- Idea: turn global attention to causal attention
- global
	- bidirectional: all frames attend to each other $O(N^2)$
- causal
	- each new frame attends to previous frames only, $O(N)$
	- previous tokens frames stored in KV cache
- each new frame is appended to the cache
- analogous to LLMs
- distill a causal student using the original as a teacher
- Full cache size:
	- $2 F L N_{h} H W d$
	- one for keys, one for values
	- $F$ number of frames
	- $L$ number of attention layers
	- $N_{h}$ number of heads
	- $HW$ number of patch tokens per frame
	- $d$ token dimension

**InfiniteVGGT**
- KV cache: each $(l, h)$ stores as separate cache
- Motivation:
	- KV cache is largely redundant
	- Tokens in adjacent frames are very similar (0.95 cosine similarity)
	- DINO backbone trained for semantics, invariant to small viewpoint changes
- prune redundant tokens from the cache
	- compute the mean key of the cache
	- calculate cosine similarity of each key relative to the mean
	- keep the top-K least similar keys and their corresponding values
	- [[Question]]:
		- seems like a rough heuristic - might be some more principled ways to do this? (entropy?)
- repeat pruning for every new received frame
	- resulting cache is a "rolling mean" of previous tokens
- first frame's tokens always kept in the cache
	- VGGT makes predictions in the coordinate system of the first frame, so it needs to be always kept
- **layer-wise cache budget**
	- not every layer gets the same cache size
	- shallow layers amplify inter-frame differences, have more diverse keys
	- very first layer and later layers are more semantically invariant
	- [[Question]]:
		- worth understanding a bit more about why this is the case
	- allow a larger cache for layers with more diverse keys
	- $p^l_{bud} = \frac{\exp{s^l_{div} / \tau}}{\sum_j{\exp{s^j_{div} / \tau}}}$, $B^l = p^l_{bud} B^{total}$
	- [[Question]]
		- what's the theory behind softmax here? average deviation - aggregate across heads? how's that done 
- [[Question]]:
	- how does redundancy affect attention logit activation?
	- is this correct: across the query distribution, we want to maximise average logit activation. Therefore we want to select a diverse range of keys. If we only select a cluster of similar keys, then we lose out on activation for some queries
	- implicit assumption: query distribution is wide/diverse

**XStreamVGGT**
- 3-stage pipeline that runs per incoming frame
	- 1) mean-pool queries of all patch tokens of the current frame $\bar{q} = \frac{1}{M} \sum_{i}{q_i}$
	- 2) estimate importance of each key, prune unimportant keys $r_j = \left< \bar{q}, k_j\right>$
		- first frame and current frame KVs are always retained in cache
	- 3) quantise the KV cache (inspiration from [[KIVI]] from LLM world)
		- per-channel quantisation for keys
			- [[Quantisation]] recap
			- some channels in key vectors are consistently larger than others
			- determine quantisation scale factor + offset for values in the same channel
		- per-token quantisation for values 
			- 

**OVGGT** ([[ECCV 2026]])
- 

**StreamCacheVGGT**

**RetrieveVGGT**


**KVTracker**
- 