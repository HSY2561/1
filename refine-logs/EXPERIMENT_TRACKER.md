# 实验执行跟踪表

更新时间：2026-09-23
服务器工程：`/hy-tmp/crack_project`

## 本轮执行状态核验（服务器回执）

本轮首次 SSH 请求返回 `Permission denied (publickey,password)`；随后使用已授权的密码认证恢复连接并取得真实回执。服务器主进程 PID 7499，SCSegamba 50 epoch 训练仍在运行，已生成 checkpoint0–16，日志已完成 epoch 16，epoch 17 已开始。官方验证口径当前最高为 epoch 14 的 0.798882。训练结束前不得把 SCSegamba 纳入正式模型排名；中途统一 test 已完成但只作趋势记录。

## 阶段状态

| 事项 | 状态 | 证据 |
|---|---|---|
| BiSeNetV2+CE 工程基线 | 已确定 | PaddleSeg 官方配置；服务器测试 mIoU 0.7606 |
| BiSeNetV2 消融 | 部分完成 | OHEM、Dice、Focal、Lovasz、EMA、增强及失败模块已有服务器日志；最佳候选测试 mIoU 0.7694 |
| 公开模型统一测试 | 已完成一轮 | OCRNet 0.7760，PP-LiteSeg 0.7661，DeepLabV3P 0.7257，SegFormer 0.7570，PIDNet-S 0.5005 |
| SCSegamba | 进行中，已实时复核 | 首轮因 0/1 标签被官方阈值 127 清成全背景而作废；0/255 标签 smoke 通过。服务器回执确认 checkpoint0–16 已生成，epoch 16 已完成、epoch 17 已开始；官方验证口径当前最高为 epoch 14 的 0.798882。中途固定阈值 test：checkpoint9 mIoU 0.758827；正式结果待 50 epoch 完成后验证集选权重 |
| MixerCSeg | 未完成，服务器环境待处理 | 服务器工程存在 `/hy-tmp/crack_project/src/MixerCSeg`；此前 selective-scan 扩展编译遇到 CUDA/PyTorch 兼容问题，尚未形成有效训练或测试结果 |
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
| S001 | SCSegamba 官方代码，错误标签编码 | train 1896 / val 348 | 作废 | 0/1 掩码被阈值 127 清成背景，loss≈0；不可排名 |
| S002 | SCSegamba 官方代码，0/255 专用标签副本 | train 1896 / val 348 | 运行中 | 计划 50 epochs；实时回执已确认 checkpoint0–16，epoch 16 已完成、epoch 17 已开始；官方验证口径当前最高为 epoch 14 的 0.798882；中途 test checkpoint9 mIoU 0.758827，仅作阶段性趋势，不作最终排名 |

中途独立 test 回执（固定阈值 0.5、全局统计、原始 test 1124）：checkpoint6/checkpoint_best mIoU 0.757861、裂缝 IoU 0.550367、P 0.693177、R 0.727623、F1 0.709983；checkpoint9 mIoU 0.758827、裂缝 IoU 0.551010、P 0.717830、R 0.703352、F1 0.710517。训练结束后仍需在验证集选权重再做正式 test。

## 下一步

1. 等待 SCSegamba 正确标签编码的 50 epoch 训练结束。
2. 在验证集用固定阈值、全局混淆矩阵统一评估 epoch checkpoint，再冻结权重与阈值；官方 `checkpoint_best` 分数不可直接与 PaddleSeg 对照。
3. 仅在冻结后用原始 test 1,124 张独立测试，并计算与其他候选相同口径指标。
4. 若可找到 CUDA 11.8 toolkit，按 MixerCSeg 官方 README 重新编译 selective-scan；否则保留为环境阻塞。
5. 在验证集基于 FP/FN、裂缝宽度和连通性做误差诊断，评估成熟模块，不用测试集调参。
6. 建立源帧互斥 split，在锁定方案后重跑最强对照、多随机种子和机器人端延迟。

## 证据路径

服务器日志：

- `/hy-tmp/crack_project/logs/recheck_bisenetv2_ce_test_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_bisenetv2_ohem_dice_ema_test_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_ocrnet_hrnetw18_test_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_ppliteseg_stdc1_test_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_deeplabv3p_r50_test_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_segformer_b0_testonly_20260924.log`
- `/hy-tmp/crack_project/logs/recheck_pidnet_s_testonly_20260924.log`
- `/hy-tmp/crack_project/logs/scsegamba_valselect_20260924.log`（作废：标签编码错误）
- `/hy-tmp/crack_project/logs/scsegamba_valselect_u8_20260924.log`（正确编码训练，进行中）

