---
layout: post
title: "Flutter BottomNavigationBar 만들어보기"
comments: true
category: flutter
description: >
   Flutter의 BottomNavigationBar에 대해 알아봅시다.
---

오늘 시간에는 Flutter BottomNavigationBar를 만들어 보겠습니다.

우선 stateful 하게 만들어보고 provider를 사용해서 stateless하게도 만들어보겠습니다.


## 1. stateful 위젯을 이용한 BottomNavigationBar 예제

처음 프로젝트를 만든 후에 main.dart 파일의 모든 내용을 지우고 처음부터 작성합니다.

~~~java
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
  }
}


class _MyAppState extends State<MyApp> {
  @override
  Widget build(BuildContext context) {
  return Container();
}

~~~

이렇게 초반 셋팅을 해놓은 후 

우선 BottomNavigationBar 부터 만들어 줍니다.


~~~java
class _MyAppState extends State<MyApp> {

  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      home: Scaffold(
        body: Container(),
        bottomNavigationBar: BottomNavigationBar(),
      ),
    );
  }
}
~~~

이렇게 MaterialApp - Scaffold 안의 한 요소로 bottomNavigationBar를 이용할 수 있습니다.

 BottomNavigationBar 안에는 다양한 요소들이 또 존재하는데 저희는 여기서 currentIndex와 onTap, items를 이용하겠습니다.


~~~java
class _MyAppState extends State<MyApp> {
  
  int _selectedIdx = 0;  // _를 붙여주면 private의 역할을 합니다. 

  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      home: Scaffold(
        body: Container(),
        bottomNavigationBar: BottomNavigationBar(
        currentIndex: _selectedIdx,
          onTap: (idx) {  // onTap은 네비게이션 바의 아이콘을 클릭했을 때 
            setState(() {   
              _selectedIdx = idx; // 그 아이콘의 index를 받아와서 selectedIdx에 값을 넣어주게 됩니다.
            });                   // 그렇게 되면 setState()가 state가 바뀌었음을 전달해주게 되고 다시 화면을 렌더링하면서 바뀐 값이 적용됩니다.
          },
          items: [
            BottomNavigationBarItem(
              icon: Icon(Icons.account_circle),
              title: Text("연락처"),
            ),
            BottomNavigationBarItem(
              icon: Icon(Icons.menu),
              title: Text("메뉴"),
            )
          ],        
        ),
      ),
    );
  }
}
~~~

이렇게 까지만 해주게 되면 BottomNavigationBar가 누르고 작동하는 것을 볼 수가 있습니다.

하지만 Body를 단순히 Container로만 구성해놓았기 때문에 아마 화면의 전환은 일어나는 것을 확인 할 수 없을텐데요.

그래서 이제는 Body를 구성할 화면을 만들어보겠습니다.

~~~java
class Contacts extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.deepPurple,
      body: Center(
        child: Container(
          child: Text("Contacts", textScaleFactor: 3.0),
        ),
      ),
    );
  }
}

class Menu extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.blueGrey,
      body: Center(
        child: Container(
          child: Text("Menu", textScaleFactor: 3.0),
        ),
      ),
    );
  }
}
~~~

 위 처럼 단순한 Contacts 화면과 Menu 화면을 만들어 보았습니다.

 그러면 이제 이것을 body에 적용을 시켜주면 되겠지요.

~~~java
class _MyAppState extends State<MyApp> {
 int _selectedIdx = 0;

 @override
 Widget build(BuildContext context) {
   List<Widget> screens = [Contacts(), Menu()]; // 아까 만들어놓은 화면 위젯들을 List에 담아줍니다.
   return MaterialApp(
     home: Scaffold(
       body: screens[_selectedIdx], // 그리고 그 위젯들을 우리가 설정해놓은 index에 맞게 보여줍니다.
       bottomNavigationBar: BottomNavigationBar(
         currentIndex: _selectedIdx,
         onTap: (idx) {
           setState(() {
             _selectedIdx = idx;
           });
         },
         items: [
           BottomNavigationBarItem(
             icon: Icon(Icons.account_circle),
             title: Text("연락처"),
           ),
           BottomNavigationBarItem(
             icon: Icon(Icons.menu),
             title: Text("메뉴"),
           )
         ],
       ),
     ),
   );
 }
} 
~~~

여기까지 완성했다면 이제 아래의 화면처럼 작동하는 BottomNavigationBar 어플리케이션을 만들게 되었습니다. (짝짝짝!)

![BottomNavigationBarApplication](https://user-images.githubusercontent.com/22094017/72127498-3d5eb180-33b3-11ea-854d-61f2750e3398.png)

여기서 한 가지 더 사용을 해보도록 하겠습니다. 바로 IndexedStack 인데요.

밑에 영상을 한 번 보시죠.

{% include video id="_O0PPD1Xfbk" provider="youtube" %}

~~~java
body: screens[_selectedIdx] 
~~~

에서

~~~java
body: IndexedStack(
    index: _selectedIdx,
    children: screens,
)
~~~

로 수정합니다. (끝)

## 2. stateless 위젯(+Provider)을 이용한 BottomNavigationBar 예제 

이번에는 StatlessWidget으로 초반 셋팅을 해줍니다.

~~~java
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
~~~

이번에는 Provider라는 것을 이용할 것이기 때문에 pubspec.yaml 파일로 이동을 해서

provider를 적어줍니다.

~~~
dependencies:
  flutter:
    sdk: flutter

  # The following adds the Cupertino Icons font to your application.
  # Use with the CupertinoIcons class for iOS style icons.
  cupertino_icons: ^0.1.2
  provider: ^4.0.1 
~~~


Provider 동작에 관해서는 따로 포스팅을 진행하겠습니다.

bottomNavigationBarProvider.dart 파일을 하나 생성해 줍니다.

~~~java
import 'package:flutter/foundation.dart';

class BottomNavigationBarProvider with ChangeNotifier {
  int _currentIdx = 0; //초기값

  get currentIdx => _currentIdx;

  set currentIdx(idx) {
    _currentIdx = idx;
    notifyListeners();
  }
}
~~~

뼈대를 만들어줍니다. 

~~~java
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Container(),
        bottomNavigationBar: BottomNavigationBar(),
      ),
    );
  }
}
~~~

가독성을 위해 따로 MainPage를 class로 분리시켜줍니다.

~~~java
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MainPage(),
    );
  }
}

class MainPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(),
      bottomNavigationBar: BottomNavigationBar(),
    );
  }
}
~~~

Provider를 이용하기 위해서 MainPage를 ChangeNotifierProvider로 감싸줍니다.


~~~java
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: ChangeNotifierProvider<BottomNavigationBarProvider>.value(
            value: BottomNavigationBarProvider(),
            child: MainPage())
    );
  }
}
~~~


이후에는 Provider로 값이 바뀌는 곳에 적절히 넣어줍니다.


~~~java
class MainPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {

    var _provider = Provider.of<BottomNavigationBarProvider>(context);

    return Scaffold(
      body: Container(),
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _provider.currentIdx,
        onTap: (index){
          _provider.currentIdx = index;
        },
        items: [
          BottomNavigationBarItem(icon: Icon(Icons.account_circle), title: Text("연락처")),
          BottomNavigationBarItem(icon: Icon(Icons.menu), title: Text("메뉴"))
        ],

      ),
    );
  }
}
~~~

이제 body만 구성하면 끝

처음과 동일하게 Contacts와 Menu라는 화면 위젯을 만들어 줍니다.

~~~java
class Contacts extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.deepPurpleAccent,
      body: Center(
        child: Container(
          child: Text("Contacts", textScaleFactor: 3.0),
        ),
      ),
    );
  }
}

class Menu extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.blueAccent,
      body: Center(
        child: Container(
          child: Text("Menu", textScaleFactor: 3.0),
        ),
      ),
    );
  }
}
~~~

List를 만들어주고 그것을 index를 이용해 body에 넣어줍니다.

~~~java
List<Widget> screens = [Contacts(), Menu()];  
body: screens[_provider.currentIdx],
~~~

이렇게까지만 해도 완성이지만 아까 배운 IndexedStack을 사용해서 마무리 지어보겠습니다.

~~~java
body: IndexedStack(
       index: _provider.currentIdx,
       children: screens,
     ),
~~~

![BottomNavigationBarApplication2](https://user-images.githubusercontent.com/22094017/72127383-cde8c200-33b2-11ea-83c3-70c813fe886b.png)

진짜 끝!

* 추가로 다양한 옵션들이 많으니 사용해보았으면 좋겠습니다.

---

 [간단한 TODO 앱](https://play.google.com/store/apps/details?id=com.woopaloopa.todo)
