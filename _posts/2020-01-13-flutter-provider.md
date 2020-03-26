---
layout: post
title: "Flutter Provider"
comments: true
category: flutter
decription: >
   Flutter의 Provider가 무엇인지, 어떻게 사용하는 것인지에 대해 알아봅시다.
---

## Provider 

### 1. Provider 란?

원래 Flutter는 BLoC(Business Logic Component) 패턴을 추천하고 있었습니다. 

(BLoC 패턴은 간단히 말해서 비즈니스 로직을 UI단과 분리하는 방식입니다.)

하지만 이 방식의 문제점이 있었습니다.

바로 사용하기에 어렵다는 것이였습니다. 그래서 Provider 패턴이 사용하기 쉽다는게 알려지면서

결국 Google IO(2019 Google IO)에서 추천되면서 큰 주목을 받았죠.

### 2. Provider를 사용하는 이유는 ?

그러면 왜 코드를 분리를 할까요?

그 이유는 여러가지가 있지만 대표적으로는 유지보수가 쉽기 때문입니다.

나중에 코드가 많아지고 복잡해지면 어떤 부분을 수정하고 싶을 때 어느곳을 수정해야될지 찾기가 어려울 수 있습니다.

코드를 분리해놓아서 예를 들어서 UI를 담당하는 코드 / 데이터를 담당하는 코드 / 통신을 담당하는 코드 등으로 나눠놓게 되었을 때는

코드를 찾는데 한결 수월해지겠죠.

그 밖에도 재사용하기에도 용이하고, 테스트 코드를 작성하기에도 수월하고 ..  

이렇게 코드를 나누는 것을 <Strong>'관심사 분리'</Strong>라고 합니다.

[Provider 공식문서](https://pub.dev/packages/provider)에서는 state를 관리하는 위젯을 사용할 때, provider를 사용하게 되면

다음의 3가지를 보장하게 한다고 합니다.

  - 유지보수
  - 테스트 가능성
  - 견고함

### 3. Provider의 종류

Provider는 다음과 같이 있습니다.

![Provider의 종류](https://user-images.githubusercontent.com/22094017/72237383-9762af80-361d-11ea-92dc-5a2f309ff122.PNG)

제일 많이 사용하게 되는 Provider는 Provider와 ChangeNotifierProvider 입니다.

다른 Provider는 점차 다뤄보도록 하고 여기서는 위의 2개만 다뤄보겠습니다.


### 4. Provider 사용해보기

```pubspec.yaml```파일안에서 provider를 추가합니다.

```
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^0.1.2
  provider: 최신버전
```







