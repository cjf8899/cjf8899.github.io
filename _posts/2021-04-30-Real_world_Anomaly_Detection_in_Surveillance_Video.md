---
title:  "Real world Anomaly Detection in Surveillance Video paper review"
last_modified_at: 2021-04-30 00:00:00 -0400
categories: 
  - Anomaly Detection
tags:
  - update
toc: true
toc_label: "Getting Started"
---

# Real world Anomaly Detection in Surveillance Video
> Waqas Sultani, et al. ["Real world Anomaly Detection in Surveillance Video"](https://arxiv.org/abs/1801.04264) Computer Vision and Pattern Recognition

# Real world Anomaly Detection in Surveillance Video 리뷰

안녕하세요. **AiRLab**(한밭대학교 인공지능 및 로보틱스 연구실) 노현철 입니다. 
제가 이번에 리뷰할 논문은 **"Real world Anomaly Detection in Surveillance Video"** 입니다.

# Abstract
-시간이 많이 걸리는 작업을 제외하여 weakly labeled로 수행한다.<br>
-Anomalous를 예측하고 학습시킨다.<br>
-새로운 데이터셋 UCF-crime을 제시한다.<br>

# Introduction
-감시카메라는 많지만 계속 모니터링은 불가능하다.<br>
-비디오 감시의 목적은 불법활동, 비정상적 사건을 탐지하는 것이다.<br>
-이상행동은 일반적으로 잘 일어나지 않아 노동력이 낭비된다.<br>
-따라서 이상을  감지하고 이 행동이 어떤 행동인지까지 분류한다.<br>
-이러한 이상행동감지는 알고리즘이 필요<br>
-현재는 폭력, 교통사고는 있지만 다른이벤트는 구별 못한다.<br>
-실제 이상행동은 복잡하고 다양하여 나열하는것은 어렵다.<br>
-따라서 minimum supervision으로 수행해야한다.<br>
-예전 sota는 초기부분에만 정상이벤트가있다고 가정, 그런다음 이상이벤트는 정상이벤트를 정확하게 재구성 할 수 없다라고 설정.<br>
-이 방식은 시간이 지남에 따라 크게 변동될 수 있으므로 오경보율이 생성.<br>
-이러한 기법은 매력적이지만 이상행동이 패턴을 가지고 있지 않아 매우 어렵거나 불가능하다.<br>
-또한 정상과 비정상의 경계가 모호하다(사람의 기준이 다르기 때문)<br>
-우리는 4가지 이슈로 정리하였다.<br>
* weakly label로 이루어진 데이터를 활용하여 이상탐지한다.(MIL Ranking loss)<br>
* 데이터셋 공개, 기존보다 25배 더 크며, 128시간으로 이루어져있다.<br>
* SOTA에 비해 우수한 성적이다.<br>
* 13가지 다은 이상행동을 인식하는 방법, C3D, TCNN을 제공한다.<br>



# Related Work

-예전에는 사람들의 움직임, 팔다리로 인간 폭력을 감지.<br>
-딥러닝의 발전으로 비디오 액션에도 몇 가지 접근법이 제안되었지만 어렵다..<br>
-우리는 weakly label로 학습하여 이상행동을 탐지한다.<br>
-SOTA를 달성한 모델은 방대한 라벨을 가지고 있다.<br>
-정상, 이상 수준의 레이블에 의존하여 학습을 한다.<br>

# Multiple Instance Learning
<img src="https://user-images.githubusercontent.com/53032349/116670449-b59f1b00-a9da-11eb-91a8-49cddf7efe8a.JPG" width="80%" height="80%" title="70px" alt="memoryblock"><br>
-위의 식을 사용하려면 많은 양의 라벨이 필요하다.<br>
-MIL은 이러한 시간적 주석이 있다는 가정을 완화한다.<br>
-포지티브(anomal)를(P1, P2, ..., Pm)까지 개별로 만들고, 이런한 인스텐스 중 하나에 이상이 있다는 것으로 가정한다.<br>
-마찬가지로 네거티브(nomal)도 같게 만들어주고, 이것은 전체가 정상인 라벨.<br>
-정확한 라벨을 알 수 없기 때문에 각 bag에서 최대점수를 밭은 인스턴스로 loss계산(아래의 식).<br>
<img src="https://user-images.githubusercontent.com/53032349/116671818-5b9f5500-a9dc-11eb-9e44-b9eebfa8f35a.JPG" width="80%" height="80%" title="70px" alt="memoryblock"><br>

# Deep MIL Ranking Model

-포지티브, 네거티브 bag에 있는 인스턴스 중 가장 높은 점수를 가진 인스턴스를 뽑으면, 포지티브에서 뽑힌 것은 이상행동일 가능성이 높다.<br>
-네거티브에서 뽑은 것은 이상행동처럼 보이지만 정상행동이다.<br>
-이 두개의 인스턴스를 서로 밀고자 한다.<br>
-힌지로스 공식에 Ranking loss를 붙힌다.<br>
-기본시간구조를 무시한다.<br>
-첫번째, 이상행동은 종종 짧은 시간에 발생한다.<br>
-두번째, 세그멘트 점수가 매끄럽게 달라야한다.<br>
<img src="https://user-images.githubusercontent.com/53032349/116672617-5ee71080-a9dd-11eb-816d-42fc1cff9710.JPG" width="80%" height="80%" title="70px" alt="memoryblock"><br>
-1은 보다 매끄럽게 만듦, 인스턴스의 희소성과  부드러움 제약을 통합<br>
-2는 오류로 false가 측정되지 않게, 이상행동이 자주 일어나지 않기 때문에 추가<br>
-각 임베딩에 별도로 레이어를 사용하는 대신 shared embedding을 사용한다.<br>
-이를 통해 계산 및 메모리가 절감하고 훈련 속도 또한 37% 향상되었다.<br>

# Experiments
<img src="https://user-images.githubusercontent.com/53032349/116673047-db79ef00-a9dd-11eb-9068-57c7352f9a20.JPG" width="80%" height="80%" title="70px" alt="memoryblock"><br>
* 프레임 크기 : 240 x 320<br>
* 속도 : 30 fps<br>
* 세그먼트(혹은 인스턴스) : 16프레임당 1개<br>
* 16프레임 -> C3D -> L2 norm -> 평균구함(4096D)<br>
* 3계층 FC에 입력(512, 32, 1)<br>
 * 사이사이 dropout 60%사용<br>
 * 첫번째, 마지막 FC레이어에 RelU, Sigmoid 적용<br>
* lr : 0.001<br>
* Adagrad optimizer<br>
* MIL Ranking loss : λ1 = λ2 = 8 x 10^-5, λ3 = 0.01<br>
-겹치지 않게 세그먼트를 32개 만듦.(겹치게 실험했지만 영향 x)<br>
-30개의 포지티브, 네거티브 백을 미니배치로 무작위 선택<br>
-Theano를 사용하여 그래프에서 자동 미분으로 기울기 계산(6,7)식<br>
-평가는 'AUC'로 평가<br>
<img src="https://user-images.githubusercontent.com/53032349/116674011-103a7600-a9df-11eb-8035-6e99114d1857.JPG" width="80%" height="80%" title="70px" alt="memoryblock"><br>
<img src="https://user-images.githubusercontent.com/53032349/116674173-47108c00-a9df-11eb-8299-aa0c9023914f.JPG" width="80%" height="80%" title="70px" alt="memoryblock"><br>
<img src="https://user-images.githubusercontent.com/53032349/116674416-95258f80-a9df-11eb-8d9c-8402213049be.JPG" width="80%" height="80%" title="70px" alt="memoryblock"><br>
