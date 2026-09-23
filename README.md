# Pavement Crack Repair Robot Research

This collaboration branch contains reproducible source code, training configurations, experiment logs, reports, and paper planning material for the pavement-crack segmentation and robot repair project.

## Current status

- Primary segmentation baseline: PaddleSeg official BiSeNetV2 + CrossEntropyLoss.
- Best BiSeNetV2 ablation so far: OHEM Cross Entropy + DiceLoss + EMA.
- Best unified Crack500 public comparison currently recorded: OCRNet-HRNet-W18, mIoU 0.7760.
- The optimized BiSeNetV2 reaches mIoU 0.7694 on the current test protocol; it must not be described as public-literature SOTA without further evidence.
- Robot work is planned around four-wheel differential-drive coarse tracking, camera/robot calibration, pixel-to-physical conversion, and three-DOF Cartesian fine positioning for repair.

## Repository layout

- `external/PaddleSeg-v2.10.0/`: selected official PaddleSeg source, configs, and tools.
- `external/PIDNet/`: selected official PIDNet source and configs.
- `PaddleSeg-release-2.8/`: project-side crack segmentation model code and Crack500 split lists.
- `experiments/crack500_baseline/`: training configs, logs, and audit notes. Checkpoints are intentionally excluded.
- `analysis/` and `refine-logs/`: planning, analysis, and experiment tracking documents.
- `PAPER_PLAN.md`, `基线模型选择报告.md`, `模型消融文档.md`: current paper and model reports.

## Reproducing experiments

1. Install PaddlePaddle for the target CUDA version and Python 3.10.
2. Obtain Crack500 separately; raw images and masks are excluded from Git.
3. Place the dataset according to the paths in the selected YAML configuration, or update the dataset root locally.
4. Run the official PaddleSeg `tools/train.py` and `tools/val.py` commands using the configs under `experiments/crack500_baseline/configs/`.
5. Record checkpoints and generated outputs outside this repository. Do not commit them.

The exact server paths and checkpoint locations in the reports are historical run references; they are not required repository paths.

## Excluded from Git

Raw datasets and images, model checkpoints and pretrained weights, local Python environments, caches, generated outputs, large archives, and credentials are excluded by `.gitignore`.

## Collaboration notes

Please update the relevant experiment tracker and report when adding a run. Keep the dataset split, random seed, input size, iteration budget, and evaluation script explicit so results remain comparable.
