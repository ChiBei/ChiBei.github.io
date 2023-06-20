---
name: 基于So-VITS-svc的音色克隆实战
tools: [Python]
image:
description: 效果还不错。
---

由于本人五音不全，遂产生使用此广受好评的开源项目克隆自己音色唱歌的想法。所有步骤主要参考其自带ReadMe，其他相关链接附在文末。

# 1. 准备工作

所用开源模型版本：So-VITS-SVC 4.1

所用解释器版本：Python 3.8.10

所用设备：NVIDIA RTX 4070 12G、NVIDIA 980 8G；需要使用NVIDIA显卡，否则会报错 Assertion Error: CPU training is not allowed。

数据集：本人录音，为一段28min的诗歌朗诵音频，方便后续切割成合适的长度音频；音频几乎无噪声，且为单人音频。

所用预先模型文件：Svc Develop Team 推荐及提供的[checkpoint_best_legacy_500.pt](https://ibm.box.com/s/z1wgl1stco8ffooyatzdwsqn2psd9lrr)

# 2. 数据集预处理

## 2.1音频分割

将原28min的音频分割为2-10s的音频，实测诗歌朗诵切出了大部分1s的音频，长度不太够，因此未来优化可以朗读一些单句更长的内容作为原音频。





