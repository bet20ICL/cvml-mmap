---
tags: [task, robotics, vla, vision-language]
---

# VLA — Vision-Language-Action Models

## Problem

Given a visual observation and a natural language instruction, output robot actions. VLAs extend vision-language models (VLMs) into the action domain, enabling robots to follow free-form language instructions grounded in visual perception.

## Paradigm

```
Image(s) → [Vision Encoder] → patch tokens
                                    │
Instruction → [Tokeniser] ──────────┤
                                    ▼
                             [LLM Backbone]
                                    │
                             [Action Head]
                                    │
                             Motor commands / waypoints
```

The vision encoder is typically a frozen pretrained model (e.g. [[SigLIP]], CLIP), whose rich representations transfer well from internet-scale pretraining to robot observations.

## Key Systems

| Model | Vision Encoder | LLM | Notes |
|---|---|---|---|
| RT-2 (Google) | PaLI-X ViT | PaLM-E | First large-scale VLA; tokenises actions as text |
| OpenVLA | SigLIP ViT-So400m | Prismatic (LLaVA-style) | Open weights; 7B params |
| pi0 (Physical Intelligence) | SigLIP | PaliGemma | Flow matching action head |
| Octo | Transformer | Transformer | Modular; multi-task |
| RoboVLMs | Various | Various | Systematic benchmark |

## Action Representations

A central design challenge: how to represent continuous actions (joint velocities, end-effector poses) in a system built for discrete tokens?

- **Discretisation / binning**: tokenise each DOF into bins, predict as text → simple but loses precision
- **Continuous action heads**: separate MLP or diffusion head on top of LLM embeddings → better for dexterous tasks
- **Flow matching (pi0)**: model action as a flow from noise to action; more expressive distribution

## Why Vision-Language Pretraining Matters

VLAs benefit from large-scale VLP (via [[Contrastive Learning]]) because:
1. The visual encoder has already learned to extract semantically rich, task-relevant features
2. The LLM backbone has strong instruction-following and generalisation
3. Robot datasets are small — freezing the encoder prevents catastrophic forgetting

## Open Problems

- **Generalisation**: to new objects, scenes, and tasks unseen at train time
- **Data efficiency**: robot data is expensive — how to leverage simulation and human video
- **Real-time inference**: LLMs are slow; action chunking and distillation help
- **Grounding**: spatial precision suffers compared to task-specific models

## Related

- [[SigLIP]] — most common visual encoder
- [[VLAs]] — MOC index
