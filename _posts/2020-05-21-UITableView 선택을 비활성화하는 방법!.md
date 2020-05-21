---
layout: post
title: "UITableView 선택을 비활성화하는 방법!"
date: 2020-05-21 22:47:22 +0530
categories: swift
---
<br>
<br>
> UITableView에서 행을 누르면 강조 표시되고 선택되는데 이것을 아무것도 하지 않게 설정하는 방법!!

<br>

<br>

정말 간단하다.

<br>

<br>

```swift
tableView.allowsSelection = false
```

<Code>tableView</code> 는 자신의 IBoutlet으로 설정한 테이블 뷰의 이름을 적으면 된다. 

<br>

하지만 Edit 모드에 있을 때에는 작동하지 않습니다. 저는 Edit 모드에 있을 때는 테이블 뷰를 수정할 수 있게끔 만들어줘야하기 때문에 생략할 예정입니다. 하지만 알아두는 것도 좋으니 어떻게 이루어지는지 봐봅시다!

<br>

```swift
tableView.allowsSelectionDuringEditing = false
```

가끔 이런 구문을 볼 때마다 이름그대로의 기능을 가지고 있어서 신기하면서도 편리하다는 생각이 드네요 :)

<br><br>

굉장히 간단하죠? 포스트도 짧네요 ...ㅎㅎ
