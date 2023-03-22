# 树叶分类竞赛

==**连清 2023.3.16**==

本次竞赛作为李沐老师“动手学深度学习”课程的课后练习，李沐老师的B站主页：https://space.bilibili.com/1567748478

## 竞赛描述

任务是预测叶片图像的类别。该数据集包含176个类别、18353个训练图像和8800个测试图像。每个类别至少有50张图像用于训练。测试集被平均分为公共排行榜和私人排行榜。

本次比赛的评估标准是“分类准确性”。

竞赛地址：https://www.kaggle.com/c/classify-leaves

## 运行环境

colab+googledrive

显卡：Tesla T4

## 主要思路

数据增强+模型融合，用了seresnext50+resnet18+resnet34，三个模型训练出来的验证集准确率分别是95%，93%，93%左右，但是模型融合后私榜测试集准确率达到了96.45%。
- 通过观察数据集，旋转、调整亮度和中心裁剪或许是不错的增强方法
- 模型融合采用了投票法，但事实上没有实现三个模型同时训练而是只训练了一个，得到csv文件后再在csv文件中进行投票（懒得写投票的代码了:-(  ）

## 踩到的一些坑

- pytorch里面label的算损失的时候会自己进行独热编码。
- 测试集不需要打乱
- 网络最好采用现成的预训练网络，自己搭的往往效果没那么好且训练比较慢
- 对于这个问题，训练时间里面图片的读取时间占了很多，包括根据路径读取图片的时间和对图片进行增强的时间
- 无法收敛的时候可以调一下学习率，可能是学习率太高了
- 抄作业是很好的学习方法
- 由于用了很多方式过拟合的技巧，过拟合其实不容易发生，可以考虑不进行验证或者降低验证的频率（原来是一轮验证一次）
- 可以把训练数据放到"/content"里面，这是colab的工作空间，如果把训练数据放到googledrive中，读取数据会非常慢（放到content中秒读，而在colab中要读几十分钟）

