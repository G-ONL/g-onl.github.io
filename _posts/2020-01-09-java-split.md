---
layout: post
title: "[Java] split 문법 "
comments: true
category: java
description: >
   java의 기본 문법 중 split에 대해 알아봅시다.
---

## split , replace 에서 dot(.) 이 적용되지 않는 이유와 해결법

leetCode 1108을 풀다보니 dot(.)에 대한 특이사항을 알게 되었습니다.

1108의 간단한 문제 소개를 드리면

input으로 1.1.1.1 또는 255.100.50.0 등 (IPv4) Ip 주소가 왔을 때 

output으로 1[.]1[.]1[.]1  또는 255[.]100[.]50[.]0 으로 나오는 것 입니다.


그래서 간단하게 Input 값에 split(".")을 줘서 나누어 보았습니다.

```java
public class main {
    
    public static void main(String[] args) throws IOException {

        String address = "1.1.1.1";
        String[] splitWord = address.split(".");
        System.out.println(splitWord.length);
    }
}
```
input으로 1.1.1.1을 넣었으니 예상 결과로는 4가 나와야겠죠.

하지만 결과 split 된 길이를 살펴보니 0이 나오게 되었습니다.

찾아보니 split() 메서드 인자로는 정규식이 들어가게 되는데 ,  dot(.)은 정규식 예약어이기 저렇게 할 경우 

dot(.)의 정규식 예약어 기능이 작동하게 됩니다.

dot(.)은 \n(개행 문자)를 제외한 모든 문자를 의미하기 때문에 split(".")을 해버리게 되면 모든 문자가 나눠지는 기준이라 다 사라지게 됩니다.

그러면 어떤 방법을 써야 할까요?

방법은 바로 ```\\.``` 입니다.

```\\``` 뒤에 특수문자가 문자 그대로를 인식하게 됩니다.

```java
public class main {
    
    public static void main(String[] args) throws IOException {

        String address = "1.1.1.1";
        String[] splitWord = address.split("\\.");
        System.out.println(splitWord.length);
    }
}
```

이렇게 해서 실행을 하게 되면 우리가 원하던 예상 결과인 4가 나오는 것을 알 수 있습니다.

