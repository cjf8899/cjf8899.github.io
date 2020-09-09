---
title:  "ResNet paper review"
last_modified_at: 2020-09-09 00:00:00 -0400
categories: 
  - Image Recognition
tags:
  - update
toc: true
toc_label: "Getting Started"
---

# Deep Residual Learning for Image Recognition
> Kaiming He, et al. "Deep Residual Learning for Image Recognition." Proceedings of the IEEE conference on computer vision and pattern recognition. 2016.

# Abstract
Deep neural networks일수록 학습이 어렵다. 그래서 residual학습으로 이 전보다 더 쉽게
만들었다. Imagenet에서 3.57%로 매우 작은 에러를 보이고 우승하였다.

# Introduction

![initial](https://user-images.githubusercontent.com/53032349/92397719-42524b80-f162-11ea-93b2-cef59272625b.png)

깊이가 증가함에 따라 overfitting이 생겨 성능이 감소한다. residual학습으로 이러한 문제를
해결할 수 있다. residual학습은 단순히 전에 파라미터를 더해주는 연산이다. 이러한 작업
만으로도 훌륭한 generalization performance를 보인다. Deep residual learning
학습과정 중 미분하는 연산 때문에 training error가 발생하기
때문에 이것을 방지하기 위해 그 전의 파라미터의 값을 더해
준다. 224*224 이미지 사용, conv다음에 BN을 사용, conv는 3*3필
터이고, stride는 2, batch size는 256, Learning rate는 0.1에
서 서서히 감소한다. weight decay는 0.0001, momentum은
0.9이고 dropout을 사용하지 않았다.

# Experiments
![initial](https://user-images.githubusercontent.com/53032349/92397925-92c9a900-f162-11ea-8718-e1f053f4b051.png)
![initial](https://user-images.githubusercontent.com/53032349/92398022-bf7dc080-f162-11ea-8d90-f27c19e4f50f.png)

34layer가 18layer보다 우수하며, shortcut으로 인해
소실 문제가 해결되었습니다. 18layer의 일반 네트워크와 18layer의 ResNet을 비
교하면 별 차이가 없다. 이유는 얕은 네트워크에서는
소실 문제가 나타나지 않기 때문이다. 네트워크가 깊어질수록 에러율이 더욱 낮아진다.

# Code Implement
ResNet code : https://github.com/cjf8899/Pytorch_ResNet



















