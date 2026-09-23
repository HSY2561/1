# 协作状态

更新时间：2026-09-23
主工作区：`D:\论文复现`  
GitHub 同步目录：`D:\github\1`  
服务器工程：`/hy-tmp/crack_project`

## 已完成

- 固定 BiSeNetV2+CE 为工程消融基线。
- 在服务器统一核验 BiSeNetV2、OHEM+Dice+EMA、PP-LiteSeg、OCRNet、DeepLabV3P、SegFormer-B0 和 PIDNet-S 的测试日志。
- 完成 BiSeNetV2 的 OHEM、Dice、Focal、Lovasz、EMA、增强和多个失败模块的筛选记录。
- 确认当前已核验测试最高为 OCRNet-HRNet-W18，mIoU 0.7760；当前 BiSeNetV2 最佳候选 mIoU 0.7694。

## 进行中

- SCSegamba 官方实现：服务器 `scsegamba` 环境，验证集选权重训练 50 epochs；实时回执已完成 epoch 16、epoch 17 已开始。官方验证口径当前最高为 epoch 14 的 0.798882；正式排名仍待训练结束后用统一协议选权重并评估原始 test。
- 训练完成后必须用原始 test 1124 独立评估，不能把官方脚本每轮验证当成最终 test。

## 阻塞与限制

- MixerCSeg selective-scan 官方扩展编译失败：服务器 nvcc 12.4，PyTorch 2.1.0+cu118。
- Crack500 裁块的源帧编号跨集合存在同源相关风险，尚未建立 source-frame-disjoint split。
- 当前没有 SOTA 结论；论文报告值与本项目固定 split 不可直接排名。

## 协作规则

1. 训练只在服务器执行。
2. 代码、配置、报告和小型结果摘要可以同步；数据集、权重、环境、密钥和大型日志禁止上传。
3. 新实验必须记录命令、配置、训练/验证/测试划分、随机种子和真实日志路径。
4. 测试集只在方案冻结后使用一次；未经复核的结果标为待核验，不写成 SOTA。

