# ML-Class-Assignment
2024机器学习基础课程大作业

## 任务描述
本学期的机器学习大作业包括四个题目：手写数字识别、医学图像检测以及图像显著性检测，任选其一完成即可。由于难度不同，每题设置了不同的最高分：

1、手写数字识别：机器学习经典问题，在灰度图像中识别10类手写数字，属于分类问题。最高分90。

2、医学图像检测：利用彩色眼底图像判断是否患病，属于分类问题。最高分95。

3、图像显著性预测：在彩色图像中，预测人眼容易关注的区域（输出显著性图），属于回归问题。最高分100。

4、彩色图像生成：设计并训练一个生成网络，生成彩色图像。可以选择使用GAN、VAE或其他生成模型，目标是从随机噪声生成与训练数据分布相似的彩色图像。最高分100分。

## 作业内容

1、在训练集上训练机器学习模型。

2、在测试集上测试模型的性能。

3、提交实验报告与模型代码。

## 题目一：手写数字识别

### 数据文件
* 训练数据集（10类，共6万个数字）：以bmp格式存储在**1-Digit-TrainSet.zip**。
* 测试数据集（10类，共1万个数字）：以bmp格式存储在**1-Digit-TestSet.zip**。

每个数据集中，每个文件名的第一个数字代表它的真实分类（label），即ground truth。

![](/1-Digit-Example.png)

### 性能指标
测试集上的分类准确度。

## 题目二：医学图像检测

### 数据文件
* 训练数据集（2类，共1639幅图像）：以jpg格式存储在**2-MedImage-TrainSet.zip**。
* 测试数据集（2类，共250幅图像）：以jpg格式存储在**2-MedImage-TestSet.zip**。

![](/2-MedImage-Example.png)

每个数据集中，以disease开头的文件为患病图像，以normal开头的文件为无病图像。

### 性能指标
最基本的指标是测试集上的分类准确度。考虑到患病与无病样本数量不均等，且两种误判（无病判断成患病、患病判断成无病）带来的风险不同，因此为了全面反映分类器性能，还可以给出精确率、AUC、ROC曲线（指标函数已给，在ROC文件夹中，代码使用详细见instruction.txt）或其他指标。

## 题目三：图像显著性预测

### 数据文件
* 训练数据集（共1600幅待检测图像及1600幅对应的显著图）：以jpg格式存储在**3-Saliency-TrainSet.zip**中。
* 测试数据集（共400幅待检测图像及400幅对应的显著图）：以jpg格式存储在**3-Saliency-TestSet.zip**中。

每个数据集中，待检测图像为人眼直接观察的彩色图像，保存在**Stimuli**文件夹；对应的显著图(即ground truth)为相同尺寸的灰度图像，颜色越亮的区域代表显著性越强，保存在**FIXATIONMAPS**文件夹。考虑到图像内容可能对结果产生影响，每个数据集都包括20种不同类型的图像，存放在20个文件夹中（如**Action**，**Affective**，**Art**……），因此分析结果时，既可以给出总体性能，又可以按类型进行分析。

![](/3-Saliency-Example.png)

### 性能指标

主观指标：预测显著图与ground truth显著图主观上对比。

客观指标：相关系数（CC）、KL散度（指标函数在metric.py文件中，可直接调用，内有使用说明），或其他衡量显著性图像相似程度的指标等。

## 题目四：彩色图像生成

### 数据文件
CIFAR-10 是一个用于图像分类任务的广泛使用的数据集，包含 10 个不同类别的彩色图片。每个类别包含 6000 张图片，总共 60000 张图片，大小为 32x32 像素，分为 50000 张训练图片和 10000 张测试图片。这个数据集在机器学习和计算机视觉任务中非常常见。

CIFAR-10 的类别包括：飞机（airplane）、汽车（automobile）、鸟类（bird）、猫（cat）、鹿（deer）、狗（dog）、青蛙（frog）、马（horse）、船（ship）、卡车（truck）。分析结果时，既可以给出总体性能，又可以按类型进行分析。

![](/4-CIFAR10-Example.png)

### 性能指标
主观指标：生成图像质量主观评价，对比数据集中的真实图像。报告中可以给出随着训练迭代轮数增加，生成图像结果的变化情况。

客观指标：使用Inception Score（IS）和Frechet Inception Distance（FID）等评价指标，分析生成图像的质量。

```python
# 首先安装torch-fidelity库
pip install torch-fidelity

import torch_fidelity
def fidelity_metric(genereated_images_path, real_images_path):
"""
使用fidelity package计算所有的生成相关的指标，输入生成图像路径和真实图像路径
isc: inception score
kid: kernel inception distance
fid: frechet inception distance
"""
  metrics_dict = torch_fidelity.calculate_metrics(
    input1=genereated_images_path,
    input2=real_images_path,
    cuda=True,
    isc=True,
    fid=True,
    kid=True,
    verbose=False
  )
  return metrics_dict
```

## 数据获取
前三个题目的数据集可以通过以下链接获取：

百度云盘下载：https://pan.baidu.com/s/1mOCFxATcCkHGbK8Vdtv5yQ

DropBox下载：https://www.dropbox.com/sh/i79cbllw6763zxg/AAA3-jPaRlYHMvsMyRbtRRmaa?dl=0

两种途径下载后文件相同，任选其一即可。


第四个题目的数据集可以通过以下代码获取：

```python
from torchvision.datasets import CIFAR10
dataset = CIFAR10(root='./CIFARdata', download=True, transform=transforms.ToTensor())
```

## 报告格式
作业报告格式包含：  
1、问题描述  
2、实验模型原理和概述  
3、实验模型结构和参数  
4、实验结果分析（包含训练集和测试集里的测试结果）,要求列举出一些**失败案例**并分析，分析指标提供越多，图表分析越详尽得分会考虑越高。  
5、总结  



