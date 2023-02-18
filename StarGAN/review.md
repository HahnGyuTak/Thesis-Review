# StarGAN 논문 리뷰

> ## 초록

* 최근 연구들은 2개의 도메인에서 이미지 변환에 효과적인 성능을 보였지만, 3개 이상의 도메인을 다루는 접근에서는 한계를 보였다.
* 이를 해결하기 위해하나의 모델을 사용하며 다중 도메인에 대한 이미지 변환을 수행하는 StarGAN을 제안한다.
* 하나의 네트워크에 있는 여러 도메인에, 여러 데이터셋(RaFD, CelebA)을 학습시킴
* 이로써 입력 이미지를 원하는 도메인으로의 유연한 변환을 수행

> ## 1. 서론

* 다른 두 도메인의 학습데이터로 a 에서 b로 변환하는 학습을 수행
* 이미지에서 여러 도메인 추출(머리색, 성별, 나이 등)
  * CelebA : 머리색, 성별, 나이 등을 추출
  * RaFD : 행복, 분노, 슬픔 등과 같은 감정 도메인 추출
* RaFD로 학습하여 CelebA 이미지에 표정을 바꾸기 위해 두 데이터셋을 함께 학습

![IMG_E790BB5F1968-1](https://user-images.githubusercontent.com/50629765/219863839-0c3d675b-a9dd-43a1-ae0c-8789f3025ac9.jpeg)

* 기존의 모델은 여러 도메인 간의 변환이 비효과적이며 비효율적이었다.
  * k개의 도메인이 존재할 경우 k(k-1)개의 generator를 학습시켜야 했기 때문
  * 각 generator는 전체 데이터셋을 모두 활용하지 못함
* StarGAN은 하나의 generatordp 여러 도메인 간의 매핑 즉, 연결 자체를 학습
  * 2개의 이미지와 도메인 정보를 입력하면 대응하는 도메인간의 변환을 학습
  * 필요없는 레이블은 무시, 데이터셋에서 추출한 특정 레이블만 학습

> ## 2. Related Work

* GAN (Generative Adversarial Networks)
* Conditional GANs
* Img-to-Img Translation (CycleGAN)
  * 하나의 모델에 두 도메인 간의 관계만 학습

> ## 3. Star Generative Adversarial Networks

## 3 - 1. Muti-Domain Image-to-Image Translation

* 다중 도메인 간 매핑을 하나의 G(generator)에 학습시키는 것이 목표
* 입력 이미지 x, 출력 이미지 y, 목표 도메인 레이블 c : [G(x, c) -> y]
* c 를 랜덤으로 생성해서 G가 유연하게 이미지를 변환할 수 있음
* 하나의 D(discriminator)가 여러 도메인을 control할 수 있도록 보조적인 분류기가 존재
* D : x → {Dsrc(x), Dcls(x)}.
  
### Adversarial Loss 

![IMG_57FC6DAEAC7A-1](https://user-images.githubusercontent.com/50629765/219863865-89884597-4d1c-4371-988f-08c97afbf0e9.jpeg)


* G는 loss 를 최소화하려하고, D는 이를 최대화하려고 한다.
  
### Domain Classification Loss

* G(x,c)로 생성된 이미지가 c로 분류되도록 하기 위해, D 맨 위에 classifier 추가
* real Img의 도메인 분류 손실을 사용하여 D를 최적화

![IMG_DD09A5BA8DC8-1](https://user-images.githubusercontent.com/50629765/219863884-b591a08f-989c-460a-b5c0-6a2e04da097b.jpeg)

* real Img의 도메인 분류 손실을 사용하여 D를 최적화
* D cls(c′|x)는 D가 계산한 도메인 레이블의 확률 분포
* D 는 이 loss 를 최소화함으로서 (real Img인 x, 레이블 c')을 분류하는것을 학습

![IMG_F4ECA0F27C1F-1](https://user-images.githubusercontent.com/50629765/219863889-f1be11ce-18cd-4b66-a968-66573beb0be1.jpeg)

* G는 생성한 이미지가 target 도메인인 c로 분류되게끔 하기 위해 이 loss를 최소화한다.

### Reconstruction Loss

* 위 loss를 최소화 한다 해도, 입력된 이미지에서 변환된 도메인을 제외하고 보존하는 것에 어려움이 있음

![IMG_72F4C97DFD50-1](https://user-images.githubusercontent.com/50629765/219863895-5c768829-1131-4f4d-9a04-8c595500e1f9.jpeg)

* cycle consistency(순환 일관성) loss 적용
* G에 변환된 이미지 G(x, c)와 원본 도메인 c'을 입력 → 원본이미지 재구성
* 총 G를 2번 사용 (원본 → 변환된 이미지 → 원본 재구성)

### Full Objective

* optimize Fuctions of G and D
  
![IMG_4F814845022B-1](https://user-images.githubusercontent.com/50629765/219863898-35a4e46b-de00-4192-b902-60bce54c374a.jpeg)

* λcls 와 λrec 는 분류, 재구성 손실에 영향을 주는 하이퍼 파라미터
* λcls 는 1, λrec 는 10 사용

## 3 - 2. Training with Multiple Datasets

* CelebA에는 머리색과 같은 속성이, RaFD에는 표정 속성이 있음
* 이와 같은 다중 데이터셋에서는 G(x, c)에서 재구성할 때 필요한 c'레이블이 필요하기 때문에 문제가 생김

### Mask Vector

* make vector를 도입, 불특정한 레이블을 무시하고 명시된 레이블에 집중

![mask_vecotr](https://user-images.githubusercontent.com/50629765/219866136-663ceab6-d40f-4080-a6fe-cc7df648216e.jpeg)


* c는 i번째 dataset의 레이블 벡터를 의미

### Training Strategy

* 다중 dataset을 학습할 때, domain 레이블은 mask vector로 정의하여 G에 입력
* G의 구조는 하나의 dataset으로 학습하는 것과 다를바 없음
* 모든 dataset의 확률분포를 만들기 위해 D의 보조 분류기를 확장
* 
