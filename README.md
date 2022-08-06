<img width="1496" alt="181875297-0143e21e-eced-4bea-ba5a-a66a11b944ba" src="https://user-images.githubusercontent.com/92352445/183236118-46275a5f-f289-470c-a729-f4efcc1b37ba.jpeg">

<div align="Right">
    project period: 2022.05.02-2022.05.13
    </br>
    </br>
    작성 일자: 2022.07.30
    </br>
    수정 일자: 2022.07.30
    </br>
    </br>
    한창훈 
    </br>
    이재서
    </br>
    이명준
    </br>
    박혜주
</div>

## 목차

1. [개요](#1-개요)

    1.1. [주제](#11-주제)

    1.2. [목적](#12-목적)

2. [서론](#2-서론)

    2.1. [배경](#21-배경)

3. [본론: 데이터](#3-본론-데이터)

    3.1. [데이터: 개요](#31-데이터-개요)

    3.2. [데이터: 내용](#32-데이터-내용)

    3.3. [데이터: 전처리](#33-데이터-전처리)

4. [본론: 모델](#4-본론-모델)

    4.1. [모델: 개요](#41-모델-개요)
    
    4.2. [모델: 내용](#42-모델-내용)

5. [본론: training-and-detecting](#5-본론-training-and-detecting)

    5.1. [training-and-detecting: map](#51-training-and-detecting-map)

6. [결론](#6-결론)

7. [참고](#참고)

8. [기타](#기타)

## 1. 개요
### 1.1. 주제
이 프로젝트는 real time human detection입니다.

### 1.2. 목적
Target을 detection함으로써 target의 이동 경로 추적하는 것이 목적입니다.

## 2. 서론
### 2.1. 배경
이 모델을 활용하여 미아, 범죄자 등 특정 인물을 원활히 추적할 수 있도록 보탬이 되고자 진행한 프로젝트 입니다.

## 3. 본론: 데이터
### 3.1. 데이터: 개요
공개되기 꺼려하는 미상의 중국 기업으로부터 실내 CCTV 50명 분, 실외 CCTV 50명 분, 총 100명분의 CCTV 동영상을 제공 받았습니다.

### 3.2. 데이터: 내용
- 각 인물당 동영상 3~5개씩 있습니다
- 동영상 속성은 1920*1080(FHD) 25fps runtime 5~10sec 입니다.

### 3.3. 데이터: 전처리
<p align="Center">
<img width="762" alt="스크린샷 2022-07-30 오후 4 11 04" src="https://user-images.githubusercontent.com/92352445/181879314-b4b21d45-265a-44d9-af66-8dd3b553731c.png">
</p>
yolov5s와 yolov5m의 input size가 640*640이므로 동영상을 640*640으로 reszie 시켰습니다.

## 4. 본론: 모델
### 4.1. 모델: 개요
Target을 detecting 하기 위해 널리 사용되고 있는 YOLOv5를 우선적으로 사용했습니다.
그리고 적은 데이터 수에도 적합한 Few Shot learning을 사용했습니다.
데이터의 수를 증강하기 위해 GAN을 사용했습니다.
다음은 이 프로젝트에서 사용한 학습 모델입니다. 

- YOLOV5
- Few Shot learning + GAN 이부분은 여기서 자세히 볼수 있습니다.(영문 주의)[더보기](https://github.com/dsjgm921/FSL_Relationnet_custom_data)

### 4.2. 모델: 내용
<p align="Center">
<img width="500" alt="스크린샷 2022-07-30 오후 3 17 41" src="https://user-images.githubusercontent.com/92352445/181877611-61732f7f-046c-4b4b-a4e6-192afb831a9b.jpg">
  </p>

YOLOv5는 object detection에서 널리 쓰이는 모델입니다. YOLOv5는 IoU(Intersection over Union)의 성능 지표를 사용합니다. 이에 대한 설명은 밑에 그림으로 대체합니다.

<p align="Center">
<img width="340" alt="스크린샷 2022-07-30 오후 4 25 40" src="https://user-images.githubusercontent.com/92352445/181879867-7e6a8106-be3a-4be4-9963-bcfa740ee9d7.png">
<img width="700" alt="스크린샷 2022-07-30 오후 3 17 41" src="https://user-images.githubusercontent.com/92352445/181879654-8f607295-aea8-4ef2-94bb-ccc91b2b1f74.png">
</p>

## 5. 본론: training and detecting

### 5.1. training and detecting: mAP
<img width="1095" alt="스크린샷 2022-07-30 오후 3 17 41" src="https://user-images.githubusercontent.com/92352445/181877459-fcdb0f2f-c525-4977-8d26-6e71a1c2e1ad.png">

- mAP가 높다 = 더 나은 detection 이 이루어지지 않았습니다.
- mAP가 높으면 오히려 overfitting 될 가능성이 높아지는 것을 확인 할 수 있습니다.
- 따라서 mAP가 80% 밑으로 나오게끔 epoch를 조절했습니다.

## 6. 결론       

</br>
<div align="Center">
  
|model|TOT_Frame|Data|batch|epoch|train_time|
|:--------:|:-----------:|:-------:|:-------:|:-------:|:-------:|
|transfer learning best|211|1, 2, 3(7:3)|64|77|2100|
|transfer learning|193|1, 2, 3(7:3)|40|77|2100|
|basic|40|1, 2, 3(7:3)|64|77|2100|
|basic|22|1, 2, 3, 5(7:3)|64|77|2100|

</div>
</br>

- 데이터를 많이 넣는다고 detecting 더 잘 되는건 아닙니다.
- batch size는 클수록 좋습니다.
- transfer learning의 엄청난 성능 확인 할 수 있었습니다.


## Refernce
[1] https://medium.com/@bjmoon.korea/ai-x-%EB%94%A5%EB%9F%AC%EB%8B%9D-fianl-assignment-84e66d7e451d

[2] https://github.com/HojinHwang/FIFA-Online4

[3] https://hojjimin-statistic.tistory.com/10

[4] http://www.gameinsight.co.kr/news/articleView.html?idxno=16078

## Source
[1] Pic Source: https://github.com/ultralytics/yolov5

[2] 
