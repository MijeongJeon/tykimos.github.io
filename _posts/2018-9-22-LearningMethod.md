---
layout: post
title:  "지도/비지도학습과 강화학습 풀이법"
author: 김태영
date:   2018-09-22 01:00:00
categories: Lecture
comments: true
image: http://tykimos.github.io/warehouse/2018-9-22-LearningMethod_1a.jpg
---

영어 문제 하나 풀어볼까요? 다음 질문에 알맞은 답을 골라 봅시다. 

Which of the following is not a mammal?
Pig
Cow
Fox
Snake

![img](http://tykimos.github.io/warehouse/2018-9-22-LearningMethod_1a.jpg)

무엇을 고르셨나요? 정답은 ‘Snake’입니다. 다른 단어에 비해 유독 생소한 단어인 ‘mammal’때문에 어려웠죠? 포유류라는 뜻이라고 합니다. 따라서 문제를 해석하면, ‘다음 중 포유류가 아닌것은?’가 됩니다. 이 문제를 풀기 전에 아래와 같은 문장들을 선행학습하였다면, 정답을 바로 아실 수 있었을 껍니다.

* Pig(돼지)는 mammal(포유류)이다.
* Cow(소)는 mammal(포유류)이다.
* Fox(여우)는 mammal(포유류)이다.
* Snake(뱀)은 reptile(파충류)이다.

![img](http://tykimos.github.io/warehouse/2018-9-22-LearningMethod_2a.jpg)

동물이름을 X라고하고 계통을 Y이라고 했을 때, ‘X는 Y이다.’라는 것을 미리 학습을 한 뒤 ‘다음 중 특정 Y가 아닌 것을?’라는 질문에는 X를 대입하면 쉽게 고를 수 있게 됩니다. 우리는 Y를 ‘라벨'이라고 하는 데요, 옷사면 붙어있는 그 라벨과 동일한 의미입니다. ‘X의 라벨은 Y이다.’라고도 말할 수 있겠네요. 이렇게 X에 대해 Y 즉 라벨에 대해서 알고 있어서 이를 학습하는 것을 **‘지도 학습(Supervised Learning)’**이라고 합니다. 각각의 X에 대해 Y이라고 지도를 해 줬으니까 말이죠. 

‘mammal’이란 의미를 몰랐지만 정답을 맞추신 분들이 계신가요? 그 분들은 다음과 같은 생각으로 맞췄을 지도 모르겠습니다. 

* Pig, Cow, Fox는 다리가 4개인데, Snake는 다리가 없어
* Pig, Cow, Fox는 새끼를 낳는데, Snake는 알을 낳아
* Pig, Cow, Fox는 3글자인데, Snake는 5글자이야

![img](http://tykimos.github.io/warehouse/2018-9-22-LearningMethod_3a.jpg)

즉 위에서 얘기한 Y(라벨)을 모른체, X의 특성만 보고 다른 하나를 골라낸 것입니다. 사전에 라벨에 대해서 지도가 없었다는 말이겠죠? 따라서 이런 방식으로 학습하는 것 **‘비지도 학습(Unsupervised Learning)’**이라고 합니다.

학습된 정보로 문제를 푸는 방식이 아닌 여러 번 문제를 풀어보면서 맞춰가는 방법도 있습니다.

* Pig를 골랐는데, -10점을 받고
* Snake를 골랐는데, +10점을 받고
* Cow를 골랐는데, -10점을 받고
* Fox를 골랐는데, -10점을 받았다

![img](http://tykimos.github.io/warehouse/2018-9-22-LearningMethod_4a.jpg)

다음에 같은 문제가 나왔을 때 여러분은 무엇을 고를까요? 점수를 많이 받은 Snake를 고르겠죠? 이 과정으로 아래와 같이 생각해볼 수 있습니다.

* 문제가 ‘상태'
* +/- 점수를 주는 것이 ‘보상'
* 보기를 고르는 것이 ‘행동'
    
즉 문제를 푸는 이(에이전트)가 현재 상태(문제)를 보고 보상을 최대한 많이 받는 쪽으로 행동(보기선택)을 취하겠금 학습을 시키는 것을 **‘강화학습(Reinforcement Learning)’**이라고 합니다.

수학문제에서 선분의 길이나 각도 문제가 나왔을 때, 수식으로 풀지않고 이미 그려진 도형에서 유추가능한 길이나 각도를 지우개에 눈금으로 표시한 뒤 지우개로 잰 기억이 납니다. 어떤 선생님은 사지선다 문제에서 번호별 정답의 개수가 동일하게 내시는 분이 계셨는데, 한 두 문제가 애매할 때는 푼 번호의 개수가 제일 적은 것으로 찍었던 기억이 나네요. 우리가 목표가 정답을 맞추는 것이라고 했을 때, 문제 정의를 어떻게 하는가에 따라 푸는 방식과 그 결과가 다릅니다. 따라서 원하는 결과가 무엇인지 명확히 알고 데이터를 분석한 뒤 어떻게 문제 정의를 할 것인가에 대한 고민 부터 하는 것이 모델 개발의 시작이라고 볼 수 있습니다.

    위 글은 (주)인스페이스 이현호님의 강화학습 세미나 발표를 기반으로 작성되었습니다. 통찰이 느껴지는 좋은 내용으로 세미나해주신 이현호님과 내용 검토와 좋은 조언을 해주신 한양대의 민규식님 감사합니다.
