---
title: "电商利器 OOTDiffusion 服装更换 本地部署，效果非常棒"
description: 
date: 2024-03-24T22:06:19+08:00
image: img_4.png
math: 
license: 
hidden: false
comments: true
draft: false
---

## 前言
最近休息了两天，在研究一些AI用于商业化方向的东西，最近找到 服装一键更换的开源项目，效果非常不错。今天就把安装步骤分享给大家，同时给大家看看真实效果。

## 部署
项目地址：https://github.com/levihsu/OOTDiffusion

没有中文的项目介绍

首先需要拉取项目的所有代码：
```shell
git clone https://github.com/levihsu/OOTDiffusion
```

基础环境搭建，我这里的显卡驱动已经安装好了， 有兴趣的同学可以参考我之前的本地部署ChatGLM2的文章安装显卡环境

机器配置是
内存 32G DDR4
显卡 2070S 8G
CPU 9700F
系统 Ubuntu22.04

显卡相关的环境解决过后就需要使用conda 安装相关的依赖，现需要启动一个Python虚拟环境
```shell
conda create -n ootd python==3.10
conda activate ootd
pip install torch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2
pip install -r requirements.txt
```

根据提示需要下载模型 ，模型下载地址有两个
```shell 
git lfs clone  https://huggingface.co/levihsu/OOTDiffusion
git lfs clone  https://huggingface.co/openai/clip-vit-large-patch14
```
下载完成后移动到主项目的 checkpoints 目录，模型非常大，需要耗费很长的时间，下载完成后效果如下：
![img.png](img.png)
接下来需要启动web 服务，不推荐使用官方的命令行。启动web服务需要修改一下run/gradio_ootd.py,需要注销掉3行代码,默认 gradio 会调用两个显卡，目前只有一个显卡，遇到报错可以找GitHub搜一下

```python
# openpose_model_dc = OpenPose(1)
# parsing_model_dc = Parsing(1)
# ootd_model_dc = OOTDiffusionDC(1)
```
启动：
```shell
python run/gradio_ootd.py
```
![img_1.png](img_1.png)

本地访问:127.0.0.1:7865:
尝试一下
![img_2.png](img_2.png)
把淘宝的衣服下载下来尝试了一下，效果真不错哦
![img_3.png](img_3.png)
![img_4.png](img_4.png)

还可以调整，随机种子，重绘次数，一次生成的个数参数，但是目前仅仅生成上半身，上面注释的就是注释掉全身替换的代码，需要两块显卡同时运行。目前电脑只有一张显卡只能这样了，而且对显卡的性能要求比较高运行了显存就占满了。

# 最后
我觉得这个效果非常炸裂，像这样的而且有一定的商业价值的项目我觉得非常有前景，对于一些想开服装网店的创业者非常有吸引力，可以节省大量的时间和金钱成本。
今天就分享到这里，下个项目在尝试声音克隆的本地部署的开源项目，关注我

如果感兴趣可以付费体验一下，但是只是限时七天，因为目前只是部署到我自己本地的电脑中，有点影响自己测试其他的项目。

只须1元赞赏解锁我的部署网址，我看下付费效果，付费需求大我可以考虑部署到云服务器上给大家玩玩。并开放了一个微信群的关键字给大家讨论

感
