---
layout: post
title: "Opensource 시작하기"
comments: true
category: opensource
description: >
Opensource 참여 순서
---

# 오픈소스 시작하기

## 오픈소스를 언제 처음 들어봤더라?

오픈소스라는 말을 처음 접한 것은 4학년 때 오픈소스라는 과목 수강하게 되면서 접하게 되었습니다.
그 과목은 오픈소스가 무엇이고, 왜 좋고 많은 프로젝트들이 나오고 있고, 발전해왔는지, 또 오픈소스 중 하나인 엘라스틱서치를 이용해 프로젝트를 진행 했습니다.
엘라스틱 서치를 사용해보는 것으로 그 과목은 마무리가 되었고, 저의 기억에서 오픈소스는 다시 잊혀져 갔습니다.

## 오픈소스에 참여하세요.

대학교에서 프로젝트를 하면서 느꼈던 것은 '아 실무에 코드들을 보고 싶다.' '진짜 직장인 개발자들은 코드를 어떻게 짤까?' 라는 환상과 배움의 목말라 있었습니다.
먼 곳에서 찾을 필요가 없다는 걸 지금에서야 느끼네요. 실력이 부족하다고 느끼면, 주변에 좋은 동료가 없다고 느끼면, 피드백을 받기 힘든 상황이면 더욱 더 오픈소스에 참여해서 실력 향상에 도움이 되었으면 좋겠습니다. 

## 오픈소스 참여 순서

### 1. 오픈소스 프로젝트 찾기

   구글 : https://opensource.google/projects/explore/featured
   ![구글오픈소스](https://user-images.githubusercontent.com/22094017/142208537-2424b81c-b170-477a-98ac-57ce65816ed8.PNG)
   넷플릭스 : https://netflix.github.io/
   ![넷플릭스오픈소스](https://user-images.githubusercontent.com/22094017/142208599-fcec2333-ab03-4753-be0b-afcbf42d7377.PNG)
   메타(구 페이스북) : https://opensource.fb.com/
   ![meta오픈소스](https://user-images.githubusercontent.com/22094017/142208609-3e54ce09-8ab7-4022-9a93-a11152779559.PNG)
   AWS : https://aws.amazon.com/ko/opensource
   ![aws오픈소스](https://user-images.githubusercontent.com/22094017/142208616-3ce687d8-c2e0-4afc-ae95-63e1aeaded35.PNG)
   라인 : https://engineering.linecorp.com/ko/opensource/
   ![라인오픈소스](https://user-images.githubusercontent.com/22094017/142208604-6cfd8247-3a46-45fc-bcd2-4744df45f273.PNG)
   네이버 : https://naver.github.io/
   ![네이버오픈소스](https://user-images.githubusercontent.com/22094017/142208568-4bf5ff18-e95c-4205-a25e-c03b11ce82ee.PNG)

   오픈소스 프로젝트를 찾기 위해서 다양한 방법이 있을 수 있겠지만, 개인적으로 저는 기업들이 만든 오픈소스들 중에서 찾아보려고 했습니다.
   몇 가지 이유는 다음과 같았습니다.
   - 기업의 필요에 의해서 만들어졌다. (기업에서 오픈소스화를 한다는 것은 분명 리소스가 나간다.)
   - 어느정도 실력이 있는 사람들이라는 개인적인 생각 (물론, 재야의 고수들도 많다.) 
   - 기업내에서 사용할 가능성이 있고, 큰 기업이니까 어느정도 사용자 수가 있다고 판단 (커뮤니티, 사용도)
   
### 2. issue / pull request / docs / CONTRIBUTING 를 보면서 분위기 흐름을 살피기
다음의 예는 Line의 오픈소스인 Armeria 입니다.

open된 이슈랑 closed된 이슈의 개수를 보면 얼마나 많은 사람들이 사용하고 있는지 확인해 볼 수 있습니다.
![armeria issues](https://user-images.githubusercontent.com/22094017/142210355-19f23ab0-19eb-47b9-9c69-67d543e7669a.PNG)

![armeria pull request](https://user-images.githubusercontent.com/22094017/142210365-9e32d5ae-2bb9-4729-92b6-b579177b4dfe.PNG)

![armeria docs](https://user-images.githubusercontent.com/22094017/142210968-41e7a4ac-411c-4733-929b-bc2c0522240b.PNG)
docs에서 예제들을 따라가며 소스를 파악하는 것이 큰 도움이 됩니다.

![Contributor manual](https://user-images.githubusercontent.com/22094017/142210681-6c2795db-a323-4160-968c-ace71716772b.png)
<strong>(중요!!) 오픈소스마다 참여하는 방법이나 룰이 모두 다르기 때문에 꼭 이러한 매뉴얼을 정독을 해야합니다.</strong>

### 3. 우선적으로 참여를 해볼만한 부분은 docs의 오타 혹은 부족한 부분을 채우는 것
제가 이번에 참여해본 오픈소스는 네이버의 fixture monkey라는 오픈소스 프로젝트이고 어떤 프로젝트인지, 이 프로젝트로 선택한 이유는 다음번 포스팅에서 진행해보겠습니다.

   ![monkey docs](https://user-images.githubusercontent.com/22094017/142211440-0f2ec854-9d56-4cd5-9a57-5768c86f95d6.PNG)
   (이 부분은 반영전에 캡쳐를 해두지 못해서 잘못된 부분이 없는 완성본입니다. 밑에 pull request에서 어떤 오타가 있었는지 보시죠)
   ![monkey docs 2](https://user-images.githubusercontent.com/22094017/142211736-a7658567-0bc2-46f7-a68e-54b4d059e544.PNG)
   (this.fixture가 현재 예제 블럭안에서는 없으므로 이 부분을 수정해주도록 합니다.)
 
### 4. 오타를 발견하면 수정하고 pull request를 날려봅시다.
   ![github com_naver_fixture-monkey_pull_89](https://user-images.githubusercontent.com/22094017/142211998-2769dd84-dc66-4f73-a860-2314865bf00b.png)
   중요. 잘못된 브랜치에 잘못 수정을 했습니다.
   마음만 급해서 발생한 문제였습니다.
   방법을 모르면 우선 issue를 열어서 물어보는 것이 먼저!
   그래도 잘못된 부분은 알려주시고, 바로 잡아주시기 때문에 부담없이 했습니다.
   ![github com_naver_fixture-monkey_pull_89 (1)](https://user-images.githubusercontent.com/22094017/142212000-a5f719ba-bb4c-4f8c-a28f-6df4ac8454d0.png)
   다시 제대로 수정 후 반영합니다.
   수정한 내용은 : this.sut -> fixture로 수정했습니다.

### 5. 피드백을 받고, 수정이나 반영을 한 후 반영되는 쾌감을 느낍니다.
   ![confirm](https://user-images.githubusercontent.com/22094017/142212639-7dad2542-780e-4895-b69d-c63cdec00a17.PNG)