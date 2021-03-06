---
layout: post
title:  "골빈해커님의 g-coin 분석 2부"
author: 김태영 작성
date:   2018-01-21 10:00:00
categories: blockchain
comments: true
image: http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_12.png
---
1부에서는 골빈해커님이 올려주신 소스코드 중 블록체인의 코어 부분에 대해서 코드리딩을 해봤습니다. 뭔가 이해를 했으니 바로 돌려보고 싶네요. 이번에는 먼저 실제로 서버를 구동시켜보고 사용해볼 예정입니다. 주로 아래 툴을 이용할 것이고 코드리딩은 3부로 미루겠습니다.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_12.png)

---

### 서버실행

아래 명령으로 간단히 서버를 실행할 수 있습니다.

    python app.py --port 5000

다음 메시지가 띄워지면 정상적으로 서버가 실행된 것 입ㄴ디ㅏ.

    * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    
웹 주소가 생겼으니 웹 브라우져에서 한 번 띄워봐야죠?

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_1.png)

아쉽게도 API만 제공하는 서버라 웹 페이지가 제공되진 않네요. 

---
### API 실행환경 만들기

골빈해커님의 깃허브에 보면 Insomnia라는 툴을 사용하라고 나옵니다. 처음 사용해보는 거지만 웹 기반 API를 쉽게 주고 받아 확인할 수 있는 툴 같네요.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_2.png)

제공되는 insomnia.json 파일을 열어보라고 합니다.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_3.png)

기대되는 군요. 오~ 차분하고 기분좋은 색감으로 왼쪽편에 명령들이 죽~ 보이네요. 중간에는 JSON 편집창이 보이고, 상단에 'Send' 버튼도 (안)보입니다. 명령을 보낸 뒤 결과는 오른쪽 창에서 볼 수 있습니다.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_4.png)

---
### 제공하는 API  인터페이스 목록

코어 소스코드로 어느정도 개념을 익혔으니 골빈해커님의 readme 정도로도 대충 유추를 할 수 있네요.

- '/transaction': 거래정보를 추가하거나 기록 대기 중인 거래정보 리스트를 받을 수 있습니다.
- '/mine' : 채굴합니다. 증명찾기를 해서 새로운 블록을 만들고 그 보상을 받습니다.
- '/node' : 새로운 노드를 만들어 이웃으로 추가합니다.
- '/chain' : 체인 검증용으로 전체 체인을 가지고 옵니다.
- '/chain/valid' : 체인을 검증합니다.
- '/chain/consensus' : 분산 노드 간 동기화를 수행합니다.

---
### 거래 해보기

먼저 readme 예제대로 해봅니다. 'POST add transaction'을 해볼까요?

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_5.png)

'신'이 골빈해커님에게 10을 주셨네요. 서버는 이 거래가 2번째 블록에 추가될꺼다라고 하네요. 아직 2번째 블록이 없는 상태이기에 블록체인이 임시적으로 가지고 있는 상황입니다. 'GET get pending transactions'으로 임시 거래정보를 확인해볼까요?

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_6.png)

하나가 있는 걸 확인할 수 있네요. 블록체인의 체인 정보를 받아보겠습니다. 'GET get full chain'을 실행하시면 됩니다.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_7.png)

빈 블록이 하나 있는 것을 볼 수가 있습니다. 채굴을 해야 블록이 하나 생기고, 생긴 블록에 거래정보를 입력할 수 있겠죠? 자 그럼 채굴해봅시다. 

---
### 채굴 해보기

'POST mining'으로 채굴을 해봅시다.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_8.png)

채굴한 결과로 새 블록에 저장된 거래정보를 보여줍니다. 근데 두 개가 보이네요. 하나는 방금 '신'이 '골빈해커'님께 보낸 건데 나머지 하나는 뭐죠? 

('드르륵 드르륵')

맞습니다. 역시 '신'께서 채굴자한테 내린 보상 (코인 1)입니다. 'GET get pending transactions'으로 아직 기록되지 않은 거래정보를 볼까요?

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_9.png)

예상대로 거래정보들이 모두 새 블록에 기록되었으므로 남아 있는게 없네요.

---
### 다시 거래 해보기

이제 거래와 채굴하는 법을 알았으니 본격적으로 거래를 해보겠습니다. 일단 이런 훌륭한 소스코드를 작성해주신 골빈해커님께 1024만큼 드리고 싶네요. 'POST add transaction'을 해보겠습니다.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_10.png)

잘 안되네요. 안되는 이유가 모죠? 블록체인의 add_transaction() 함수에서 통장(book)의 check_balance() 함수를 호출하죠? 여기서 보내는 이가 유효하지 않거나 잔고가 없으면 에러를 띄웁니다. 'tykimos'이란 계정 ID는 아직 사용한 적이 없어서 안되겠네요. 일단 '신'으로부터 1024만큼 받아야겠습니다.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_11.png)

거래는 성사되었지만 아직 거래정보가 통장에 기록이 되지 않은 상태라서, tykimos 계정에 코인은 없는 상태입니다. 그럼 거래정보를 등록하려면 어떻게 해야하죠?

맞습니다. 채굴자가 새로운 블록을 만들어야죠. 'POST mining' 해보겠습니다. 잘 되네요. proof이 62입니다. 증명을 찾기 위해 62번을 시도했나봅니다. 채굴자도 덤으로 코인하나를 받았네요.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_12.png)

채굴자 ID가 '531787c118db4bd7b7777adec0fe29cb'인가 본데, 지금까지 코인이 두 개 있는 것 같습니다. 조금 있다가 이 코인을 받아보도록 하죠. 이제  tykimos 계좌에 1024만큼 코인이 있을테니 골빈해커님께 보내보도록 하겠습니다.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_13.png)

오~ 거래가 성사되었네요. 다음 신규 블록에서 기록이 될꺼라는 메시지가 나옵니다. 채굴자가 가진 코인 두 개를 뺏어봅니다.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_14.png)

잘 되네요.

여기서 궁금한 것이 생겼습니다. 거래정보가 통장에 아직 기재가 되지 않은 상태이기에 골빈해커님에게 1024를 더 보내볼까요? 왠지 될 것 같은데요. 아직 통장에 기재가 안된 상태이기 때문에 송금 내역을 체크할 수가 없는 것 같습니다.

![img](http://tykimos.github.io/warehouse/2018-1-21_g-coin_analysis_part2_15.png)

음. 예상대로 송금해버리네요. 무언가 처리가 필요할 것 같습니다.

---

### 요약

2부에서는 서버를 실행시킨 후 Insomnia을 통해서 몇가지 테스트를 해봤습니다. 어떤 시나리오로 돌아가는 지 대충 이해가 되네요. 3부에서는 남은 소스코드 마져 리뷰하고, 멀티노트 테스트를 해보겠습니다.

---

### 같이 보기

* [골빈해커님의 g-coin 분석 1부](https://tykimos.github.io/2018/01/21/g_coin_analysis_part1/)
* [골빈해커님의 g-coin 분석 2부](https://tykimos.github.io/2018/01/21/g_coin_analysis_part2/)
* [골빈해커님의 g-coin 분석 3부]
