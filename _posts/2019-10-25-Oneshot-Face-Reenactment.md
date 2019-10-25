## 1. Introduction
- Face Reenactment의 조건을 설명함
a. Photo-realistic face 
b. pose & expression Seqeunce를 자연스럽게 만들어야함
c. One shot or Few shot으로
- Problem setting
a. 모든 부분적인 View에 있어서 한 사진만을 가지고 생성할 수 있어야하는 문제
b. 1개의 이미지는 1개의 pose&expression을 가지고 있기 때문에 그것을 다른것으로 변형하는데 커버해야할 부분에 대한 문제
c. Identity preserving
- Key
a. Disentagle
b. composing appearance and shape information
- Goal
a. 얼굴 확인과 같은 어려운 작업에서 배운 얼굴 표현은 좋은 후보로 보입니다. 그러나 우리의 실험은 저수준 텍스처 정보를 잘 유지할 수 없다는 것을 보여줍니다. (뭔말일까)
b. face reconstruct를 할 때 pose와 expression을 배제하고 해야한다는 것 (Shape-independent face appearance)
c. natural gap이 test와 training사이에 존재함. 이유는 training과 test에 사용되는 이미지가 다르니깐 당연한 것

2. RW는 넘어감
3. Approach

- Overview
§ input face x를 F : X -> A(appearance) 와 E : X -> B(representation) 로 출력
§ Decoder를 통해 D : B x A -> X(target)을 출력
a. Disentangle-and-Compose Framework
- 앞서 말한대로 Face parsing은 E(face pose Encoder)로 겉모습(appearance)는 F로
- 인코더 F에서 Decoder D로 Concat해주는 방식을 이용함 (for reduce information loss )
- SPADE(SPatially Adaptive DEnormalization) 블럭을 추가하여 adaption에 민감하게 반응함
- Shape Encoder(E)
□ Shape boudary를 학습한 모델로 얼굴모양(표정)을 segment로 추출 (WFLW dataset으로 학습했다고...)
□ Gaze(응시,눈깔) channel을 추가적으로 넣어 학습해서 segment 추출(EOTT dataset으로 학습)
□ 다른 모듈(D,F)가 학습중일 때, 이건 pre-trained라서 걍 내비둔다.
- Appearance Auto-Encoder(F)
□ Appearance Encoder(이하 F)에서는 1장의 이미지에서 표면적인 정보들을 학습한다(identity& local information)
□ x_t(F의 출력물)의 모습을 갖추게끔 학습하면서 중간중간 레이어에서 D에 concat
□ F는 shape와 independent하다 (이거 왜자꾸 나오는걸까?)
- Semantically Adaptive Decoder(D)
□ SPADE논문에서 나온거처럼 Adaption을 의식하고 SPADE block을 사용함. 그것에 대한 설명
□ 추가적으로 Pix2PixHD (이렇게되면 인용을 어디에 달아야해?)에서 나온 Multi-scale방식으로 decode
b. FusionNet

- 이미지 기반으로만 한다면 주변의 배경때문에 blurry한 결과가 나온다
- Shape 기반으로만 한다면(Segment에서 표현된 얼굴형 위주) 수염이나 주름같은 디테일이 깎인다.
- 결국 이 두 문제를 해결하기 위해 두가지 이미지 생성을 fusion하는 방식으로 결과를 냈다.

