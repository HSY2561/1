# 实验执行跟踪表

更新时间：2026-09-23  
服务器工程：`/hy-tmp/crack_project`

## 阶段状态

| 事项 | 状态 | 证据 |
|---|---|---|
| BiSeNetV2+CE 工程基线 | 已确定 | PaddleSeg 官方配置；服务器测试 mIoU 0.7606 |
| BiSeNetV2 消融 | 部分完成 | OHEM、Dice、Focal、Lovasz、EMA、增强及失败模块已有服务器日志；最佳候选测试 mIoU 0.7694 |
| 公开模型统一测试 | 已完成一轮 | OCRNet 0.7760，PP-LiteSeg 0.7661，DeepLabV3P 0.7257，SegFormer 0.7570，PIDNet-S 0.5005 |
| SCSegamba | 进行中 | 官方代码；验证集选权重训练已启动，不能把 smoke 结果计入排名 |
| MixerCSeg | 阻塞 | 官方 selective-scan 扩展编译时 nvcc 12.4 与 PyTorch cu118 不一致 |
| SOTA 验收 | 未完成 | BiSeNetV2 最佳候选低于 OCRNet，强模型协议仍需统一 |
| 源帧互斥 split | 未完成 | 当前裁块清单存在同源拍摄编号跨集合风险 |

## 已核验运行

| Run ID | 变体 | 数据 | 状态 | 结果 |
|---|---|---|---|---|
| B001 | BiSeNetV2 + CE | Crack500 test 1124 | 完成 | mIoU 0.7606 |
| B002 | BiSeNetV2 + OHEM + Dice + EMA | Crack500 test 1124 | 完成 | mIoU 0.7694 |
| C001 | OCRNet-HRNet-W18 | Crack500 test 1124 | 完成 | mIoU 0.7760 |
| C002 | PP-LiteSeg-STDC1 | Crack500 test 1124 | 完成 | mIoU 0.7661 |
| C003 | DeepLabV3P-ResNet50 | Crack500 test 1124 | 完成 | mIoU 0.7257 |
| C004 | SegFormer-B0 | Crack500 test 1124 | 完成 | mIoU 0.7570 |
| C005 | PIDNet-S | Crack500 test 1124 | 完成但失败表现 | mIoU 0.5005，裂缝 Recall 0.0554 |
| S001 | SCSegamba 官方代码 | train 1896 / val 348 | 运行中 | 50 epochs，验证集选 checkpoint |

## 下一步

1. 等待 SCSegamba 验证集选权重训练结束，外部稳健评估其原始 test 1124。
2. 取回 SCSegamba 完整日志、checkpoint 和独立测试指标。
3. 若可找到 CUDA 11.8 toolkit，按 MixerCSeg 官方 README 重新编译 selective-scan；否则保留为环境阻塞。
4. 在验证集基于 FP/FN、裂缝宽度和连通性做误差诊断，优先复查已经证明有效的 OHEM+Dice+EMA，不再盲目叠加失败模块。
5. 建立源帧互斥 split，在锁定方案后重跑最强对照、多随机种子和机器人端延迟。

## 证据路径

服务器日志：

- `/hy-tmp/crack_project/logs/recheck_bisenetv2_ce_test_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_bisenetv2_ohem_dice_ema_test_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_ocrnet_hrnetw18_test_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_ppliteseg_stdc1_test_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_deeplabv3p_r50_test_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_segformer_b0_testonly_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_pidnet_s_testonly_20260924.log`
- `/hy-tmp/crack_project/logs/scsegamba_valselect_20260924.log`

