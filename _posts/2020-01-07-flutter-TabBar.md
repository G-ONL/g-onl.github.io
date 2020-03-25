---
layout: post
title: "Flutter TabBar"
comments: true
category: flutter
description:  >
   Flutter TabBar에 대해 알아봅시다.
---

## 공식문서 번역 - Flutter TabBar

[공식문서](https://api.flutter.dev/flutter/material/TabBar-class.html)

---


* 공식문서를 번역한 것입니다. 틀릴 수 있기 때문에 틀린 부분은 댓글에 남겨주시면 반영하겠습니다.

* 명확하지 않은 부분은 (?)를 쳐놓고 영문을 적어놓았습니다.

---

#### 설명

TabBar는 탭들이 가로줄로 표시된 Material 디자인 위젯이다.



TabBar는 일반적으로 AppBar 안에 AppBar.bottom안에서 만들어져 사용되며, TabBarView와 함께 사용이 된다.



만약에 TabController를 제공하지 않으면, 대신에 DefaultTabController를 제공한다. TabController의 TabController.length는 tab들의 개수와 TabBarView.children의 개수와 동일해야 한다.



Material 위젯이 상위에 있어야 한다. (?)

(Requires one of its ancestors to be a Material widget.)



현재 context에서 설정된 경우 TabBarTheme의 값을 사용합니다.



구현된 샘플을 보려면, TabController 문서를 확인하세요.

---

#### (Inheritance) 상속/계층

Object > Diagnosticable > DiagnosticableTree > Widget > StatefulWidget > TabBar

---

#### (Implemented types) 구현된 Types

PreferredSizeWidget

---

#### TabBar의 Constructor(구조)

```java
TabBar({Key key, 
	@required List<Widget> tabs, //필수값 tab들을 나열하면 된다.
	TabController controller,
	bool isScrollable: false, //tab을 옆으로 움직일 수 있는가
	Color indicatorColor,
	double indicatorWeight: 2.0,
	EdgeInsetsGeometry indicatorPadding: EdgeInsets.zero,
	Decoration indicator, 
	TabBarIndicatorSize indicatorSize,
	Color labelColor,  //label의 색깔
    TextStyle labelStyle, // label의 글 스타일
	EdgeInsetsGeometry labelPadding,
	Color unselectedLabelColor, // 선택되지 않은 탭의 label의 색깔
	TextStyle unselectedLabelStyle, // 선택되지 않은 탭의 label의 글 스타일
	DragStartBehavior dragStartBehavior: DragStartBehavior.start,
	ValueChanged<int> onTap })
```




