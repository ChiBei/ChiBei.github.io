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

## 2.1 音频分割

将原28min的音频分割为2-10s的音频，实测诗词朗诵切出了大部分1s的音频，长度不太够，因此未来优化可以朗读一些单句更长的内容作为原音频。总计分割出342个短音频。

本项目不自带音频分割功能，因此使用Audio Seg文件来进行分割，只需要更改输入输出路径即可。

## 2.2 重采样

由于我直接使用Adobe Audition录制，录制时直接选定参数为采样率为44100HZ。但是还是运行了重采样resample文件，因为还包含转换声道、响度匹配等功能。

## 2.3 训练集验证集分割

运行preprocess_flist_config文件。

## 2.4 生成hubert和f0

运行preprocess_hubert_f0文件。

# 3. 训练

运行train文件。

音频长度要足够短，过长会直接显存爆炸。

训练中途不能主动停止，即按下Ctrl+C，否则再次重启时会从头训练；当因为种种报错停止时，修正后重启则会在原有基础上继续训练。

# 4. 推理

下载到的音乐文件需要进行伴奏和人声分离，可以使用 demucs：

```shell
demucs --mp3 --two-stems=vocals "H:\ProjectsFiles\music\wfyl.mp3"    
```

 mp3转wav文件可以直接使用 ffmpeg：

```shell
ffmpeg -i "H:\ProjectsFiles\vocals.mp3" -f wav "H:\ProjectsFiles\vocals.wav"
```

得到wav源音乐文件，既可以替换音色了。

训练完成的模型存储在logs/44k/路径下，待转换wav文件位于raw路径下。

```shell
python inference_main.py -m "logs/44k/G_30400.pth" -c "configs/config.json" -n "vocals.wav" -t 0 -s "nen"
```

有时因为待转换wav文件过长也会出现显存爆炸，此时可以使用 ffmpeg 切分一下，后期可以再拼起来：

```shell
ffmpeg -i "H:\ProjectsFiles\vocals.wav" -ss 00:00:00  -t  00:02:00 "H:\ProjectsFiles\vocals_1wav"
```

此时即得到替换结果，实测结果还不错。



文章还有很多细节未写全，因为项目中间经历了换显卡等诸多事情，不是做完立即总结的，需要改进。

参考链接：

[So-VITS/VITS炼丹速通指南](https://zhuanlan.zhihu.com/p/618630799)

[So-VITS/VITS他人笔记](https://www.bilibili.com/read/cv23229977)

[So-VITS/VITS常见报错](https://www.bilibili.com/read/cv22206231/)

[Demucs使用教程](https://zhuanlan.zhihu.com/p/510755328)

[使用GPU训练的PyTorch版本下载](https://ibm.box.com/s/z1wgl1stco8ffooyatzdwsqn2psd9lrr)

[Torch的whl文件仓库](https://download.pytorch.org/whl/torch/)

[wav文件编码问题](https://blog.csdn.net/Ninja_Cat/article/details/109152416)

[cv2安装问题](https://blog.csdn.net/lq18804095672/article/details/105096231)

