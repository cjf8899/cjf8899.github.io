---
title:  "RetinaFace paper review"
last_modified_at: 2021-01-14 00:00:00 -0400
categories: 
  - Face Detection
tags:
  - update
toc: true
toc_label: "Getting Started"
---

# RetinaFace
> Jiankang Deng, et al. ["RetinaFace: Single-stage Dense Face Localisation in the Wild"](https://arxiv.org/abs/1905.00641) Computer Vision and Pattern Recognition

# RetinaFace 리뷰

안녕하세요. **AiRLab**(한밭대학교 인공지능 및 로보틱스 연구실) 노현철 입니다. 
제가 이번에 리뷰할 논문은 **"RetinaFace: Single-stage Dense Face Localisation in the Wild"** 입니다.

# Abstract
-정확하고 효율적인 얼굴 위치 파악은 여전히 어려운 과제이다.<br>
-single-stage face detector인 RetinaFace를 제시한다.<br>
-5 가지 측면에서 기여한다.<br>
* 5개의 얼굴 랜드마크로 상당한 개선이 되었다.<br>
* 3D 모양 얼굴 정보를 예측하기 위해 자체적으로 디코더 브랜치를 추가<br>
* 이리하여 sota 1.1%차이로 91.4% AP를 달성하였다.<br>
* IJB-C test set에서 ArcFace(loss)를 사용하여 결과를 개선 할 수 있다. (TAR=89.59% for FAR=1e-6)<br>
* 가벼운 백본을 사용함으로써 cpu코어에서 실시간으로 동작이 가능하다.<br>

# Introduction
-Automatic face localisation은 얼굴 속성 및 얼굴 정체성 인식과 같은 많은 응용분야에서 얼굴 이미지 분석의 전제조건이다.<br>
-face localisation의 좁은 정의는 얼굴 bounding boxes이지만 우리는 넓은 정의로 얼굴 검출, 얼굴 정렬, 픽셀 별 얼굴 분석 및 3D 밀집 대응 회귀등으로 참조한다.<br>
-이러한 dense face localisation은 모든 다른 척도에서 정확한 얼굴 위치 정보를 제공한다.<br>
-최근 딥러닝의 발전으로 face detection도 영감을 받아 발전되었다.<br>
-일반 물체와 달리 얼굴의 비율은 (from 1:1 to 1:1.5) 이지만 스케일의 차이가 심하다.<br>
-가장최근 sota는 singlestage에 초점을 맞추고 있으며, feature pyramids를 이용하여 위치와 스케일을 조밀하게 샘플링하여 성능이 좋다.<br>
-이러함을 따라 single-stage face detection framework를 개선하고 multi-task losses를 사용하여 아이디어를 제시한다.<br>

<img src="https://user-images.githubusercontent.com/53032349/104598027-6b0ec280-56b9-11eb-978c-5c78d2aa4cec.png" width="80%" height="80%" title="70px" alt="memoryblock"><br>

-five facial landmarks는 이전 논문에서 사용하여 영감받아 사용하였다. test 데이터에 제한으로 부분의 데이터 셋에서는 검증하지 못했지만 WIDER FACE hard test set에서는 최고성능을 자랑했다.<br>
-self-supervised 3D morphable models이 요즘 real-time으로 달성하였다. <br>
-Mesh Decoder는 shape and texture를 활용하여 real-time speed이상을 달성하였다.<br>
-본 논문은 픽셀단위의 3D얼굴 형태를 예측하기위해 supervised된 메시 디코더를 사용한다.<br>

-요약하자면<br>
* face score, face box, five facial landmarks, and 3D position를 사용하는 방법을 제안<br>
* On the WIDER FACE hard subset, RetinaFace outperforms the AP of the state of the art two-stage method (ISRN [67]) by 1.1% (AP equal to 91.4%).<br>
* RetinaFace는 ArcFace의 정확도를 개선하는데 도움을 줌. 얼굴인식을 크게 향상시킴<br>
* RetinaFace can run real-time on a single CPU core for a VGA-resolution image.<br>
* Extra annotations and code have been released to facilitate future research.<br>

# Related Work
<img src="https://user-images.githubusercontent.com/53032349/104598236-af01c780-56b9-11eb-9c2a-3a34fcf21d43.png" width="80%" height="80%" title="70px" alt="memoryblock"><br>
-예전엔 Image pyramid가 자주 사용하였지만, feature pyramid, sliding-anchor on multi-scale feature maps가 빠르게 우세하였다.<br>
-Two-stage는 Faster R-CNN등이 사용하였고, 높은 정확도가 특징이다.<br>
-single-stage는 unbalanced positive and negative samples during training이 학습되는데 sampling and re-weighting methods가 불균형을 처리하였다.<br>
-Two-stage에 비해 single-stage가 더 효율적이고 재현율이 높지만 정확도는 떨어질 수 있는 위험이 크다.<br>
-작은얼굴을 캡쳐하기위해 SSH and PyramidBox를 feature pyramids에 적용시켜 receptive field from Euclidean grids를 확대 시켰다.<br>
-DCN은 geometric 가능한 레이어를 사용하였다.<br>
-rigid (expansion) and non-rigid (deformation) context modelling를 향상시키기 위해상호보안적이고 직교함을 나타낸다.<br>

# RetinaFace

## Multi-task Loss
<img src="https://user-images.githubusercontent.com/53032349/104598134-8f6a9f00-56b9-11eb-8eea-02ec04ae0656.PNG" width="80%" height="80%" title="70px" alt="memoryblock"><br>
-The classification loss Lcls is the softmax loss for binary classes (face/not face).<br>
-Face box regression loss Lbox는 (smooth-L1)<br>
-Facial landmark regression loss Lpts<br>
-Dense regression loss Lpixel <br>
- λ1-λ3은 0.25, 0.1 및 0.01로 설정되어있으며, 랜드마크에 가중치를 더욱 주고 있다.<br>

## Dense Regression Branch
- joint shape and texture decoder를 가진 Mesh Decoder를 사용하였다.<br>

<img src="https://user-images.githubusercontent.com/53032349/104598243-b0cb8b00-56b9-11eb-99af-4587e9269dad.png" width="80%" height="80%" title="70px" alt="memoryblock"><br>


