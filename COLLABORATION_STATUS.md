# Collaboration status

This branch is the lightweight, reviewable GitHub snapshot of the pavement crack repair robot project. It is intentionally separate from the original `main` history, whose initial commit contains local datasets, environments, checkpoints, and generated artifacts.

## Research question

The project combines pixel-level pavement crack segmentation with a mobile repair robot: the four-wheel differential-drive chassis performs coarse crack following, then the camera and three-DOF Cartesian repair platform perform calibrated fine positioning and repair.

## Verified segmentation results

All values below are copied from the current reports and use the Crack500 test split of 1,124 images under the recorded PaddleSeg evaluation protocol.

| Model | Test mIoU | Crack IoU | Precision | Recall | F1 | Role |
|---|---:|---:|---:|---:|---:|---|
| BiSeNetV2 + CE | 0.7606 | 0.5545 | 0.7146 | 0.7122 | 0.7134 | Main baseline |
| SegFormer-B0 | 0.7570 | 0.5466 | 0.7388 | 0.6776 | 0.7069 | Public comparison |
| PIDNet-S | 0.5005 | 0.0547 | 0.8036 | 0.0554 | 0.1036 | Public comparison |
| PP-LiteSeg-STDC1 | 0.7661 | 0.5634 | 0.7527 | 0.6915 | 0.7219 | Public comparison |
| DeepLabV3P-ResNet50_vd | 0.7257 | 0.4955 | 0.5995 | 0.7406 | 0.6619 | Public comparison |
| OCRNet-HRNet-W18 | 0.7760 | 0.5819 | 0.7601 | 0.7128 | 0.7356 | Current public comparison upper bound |
| BiSeNetV2 + OHEM + Dice + EMA | 0.7694 | 0.5705 | 0.7296 | 0.7233 | 0.7264 | Best BiSeNetV2 ablation |

The optimized BiSeNetV2 is currently below OCRNet-HRNet-W18 by 0.0066 mIoU. Do not claim public SOTA until a future run exceeds the same reference under a matched protocol.

## What to run next

1. Recreate the PaddlePaddle/PaddleSeg environment on the target machine.
2. Download Crack500 separately and keep it outside this repository.
3. Reproduce the public-model comparison from the YAML files and the experiment report.
4. Continue BiSeNetV2 ablations only with documented, mature PaddleSeg components.
5. Add robot-side calibration, centerline extraction, chassis tracking, three-DOF positioning, and repair metrics after the vision protocol is fixed.

## Data and artifact policy

The branch contains source, configs, logs, Markdown reports, split-list metadata, and diagrams. It excludes raw images and masks, local environments, checkpoints, pretrained weights, generated images/videos, caches, archives, and credentials. Use Git LFS or a separate artifact store if the team later decides to share weights.
