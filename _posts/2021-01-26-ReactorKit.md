---
title: "ReacotrKit 예제 따라하면서 뿌시기!"
date: 2021-01-26 19:26:28 -0400
categories: ReactorKit 
---
toc: true
toc_label: "In Post"
toc_icon: "cog"
---

![image](https://user-images.githubusercontent.com/49550838/105825875-6c1de900-6003-11eb-9826-158cbfd7b82c.png)



### ReactorKit이란?

 반응형 단방향 앱을 위한 프레임워크로 StyleShare, Kakao를 비롯한 여러기업에서 사용하고 있는 기술입니다. Flux와 Reactive programming의 개념을 결합하여 만들어진 ReactorKit은 사용자 인터랙션과 뷰 상태가 관찰 가능한 스트림을 통해 단방향으로 전달됩니다. 뷰와 비즈니스 로직을 분리할 수 있게 되면서 모듈간 결합도가 낮아지고 테스트하기 쉬워졌다고 합니다.



### ReactorKit의 데이터 흐름

 전체적인 흐름은 View와 Reactor라는 개념을 통해 존재합니다. 

1. View는 Action을 Reactor에게 전달합니다.(View는 비즈니스 로직을 수행하지 않음!)
2. Reactor는 전달받은 Action에 따라 비즈니스 로직을 수행합니다.
3. 그 후 Reactor는 상태(State)를 변경하여 View에게 전달합니다.

__여기서 View는 View Controller나 Cell 등을 말합니다__



ReactorKit의 데이터 흐름을 구체화 시킨다면 다음의 사진입니다. Action이 들어오면 두 단계를 거쳐서 State를 변경합니다. 

![image](https://user-images.githubusercontent.com/49550838/105826014-9e2f4b00-6003-11eb-98be-50703b9fbdeb.png)

- **Mutate()** 는 Action 스트림 ---> Mutation 스트림으로 변환하는 역할

  보통 네트워킹이나 비동기 로직 등의 사이드 이펙트를 처리합니다.

- **Reduce()** 는 Mutation에서 방출된 값을 받아서 상태를 반환합니다.



### View

! View 프로토콜을 적용하면 View를 정의할 수 있습니다.

- **DisposeBag**속성과 
- **bind(reactor: )** 메서드

필수로 필요합니다.



### Reactor

! Reactor 프로토콜을 사용하여 정의할 수 있습니다

- 사용자 인터랙션을 표현하는 **Action**
-  뷰의 상태를 표현하는 **State** 
- 상태를 변경하는 가장 작은 단위인 **Mutation**
- 가장 첫 상태를 나타내느 **initialState**

필수로 필요합니다.



### ReactorKit을 써야하는 이유?

1. MVVM 구조는 개발자마다 바인딩 방법이 다른데 프레임워크로 강제되는 사용법으로 명확성을 찾을 수 있습니다.
2. 가독성이 좋아 협업에도 적합합니다.
3. 기존 앱이 이미 존재하는 경우에도 작은 단위부터 ReactorKit을 도입하여 리팩토링이 수월합니다.



### 예제로 보는 실전 연습!

https://github.com/ReactorKit/ReactorKit 에 기재된 Examples를 보며 ReacotrKit을 익혀보겠습니다.

GitHubSearch라는 예제입니다.



#### 1. Reactor 만들기

<img width="638" alt="image" src="https://user-images.githubusercontent.com/49550838/105826037-a7201c80-6003-11eb-9c43-341b88230efb.png">

아까 설명했듯이 Reactor 프로토콜을 사용하여 만들어주었습니다. GitHub를 검색하기 위한 구조로 안에 값을 채워주겠습니다.



<img width="576" alt="image" src="https://user-images.githubusercontent.com/49550838/105826086-b1dab180-6003-11eb-8658-f7e09137feaa.png">

1. Action

   updateQuery는 검색한 String을 지칭합니다

   loadNextPage는 무한스크롤을 위해 만들어졌습니다

2. **Mutation**

   setQuery는 비즈니스 로직으로 들어갈 쿼리값을 의미합니다

   setRepos는 검색한 결과인 GItHub의 Repository 데이터를 담습니다

   appendRepos는 무한 스크롤로 받은 새로운 데이터들을 append해줍니다

   setLoadingNextPage Next Page를 로딩할 수 있는지 여부 확인

3. **State**

   query는 현재 검색한 String

   repos는 검색 결과 데이터

   nextPage는 페이지 수

   isLoadingNextPage는 로딩 확인



그 다음엔 Action 스트림을 Mutation 스트림으로 바꾸기 위해 mutate()로 구현해보겠습니다

<img width="743" alt="image" src="https://user-images.githubusercontent.com/49550838/105826190-ce76e980-6003-11eb-825e-4ca3e86193b4.png">

1. updateQery를 이용해서 현재 SearchBar에 어떤 String이 찍히고 있으며 그 String에 대한 검색 API를 이용합니다. 그리고 Repository 데이터를 담는 setRepos에 검색 결과를 담아 Mutation에 담습니다.
2. loadNextPage는 무한 스크롤을 사용하기 위해 다음 페이지를 로딩합니다.





그리고 reduce()를 구현해봅시다.

<img width="548" alt="image" src="https://user-images.githubusercontent.com/49550838/105826272-e77f9a80-6003-11eb-85ec-ffe1589c5600.png">

Mutation에서 전달된 상태를 받아서 방출하는 것이 reduce()의 역할이다. 각 Mutation의 구조들을 State로 방출해주고 있는 모습을 볼 수 있다.



이 글은 ReactorKit을 중점적으로 다루고 있기 때문에 다른 비즈니스 로직은 생략하도록 하겠습니다.



#### 2. Reactor의 View 만들기

앞서 View를 만들 때 View라는 프로토콜을 명시해야한다고 설명했었습니다. 만약에 Storyboard를 사용하여 UI를 구성할 경우엔 View 프로토콜이 아닌 **StoryboardView 프로토콜**을 사용하여 바인딩해줘야합니다.



<img width="643" alt="image" src="https://user-images.githubusercontent.com/49550838/105826300-f1090280-6003-11eb-8502-57428e956d52.png">

필수로 DisposeBag과 bind()를 구현한 모습을 볼 수 있습니다. reactor의 타입은 아까 만들어준 Reactor의 이름을 붙이면 됩니다.



<img width="573" alt="image" src="https://user-images.githubusercontent.com/49550838/105826398-0aaa4a00-6004-11eb-958e-8bc8a7f3655e.png">

Action부터 바인딩 해봅시다.

설정한 Action은 updateQuery와 loadNextPage입니다. 

1. searchBar의 text를 reactor의 action에 바인딩

2. tableViewdml height를 계산해서 reactor의 action에 바인딩



<img width="617" alt="image" src="https://user-images.githubusercontent.com/49550838/105826452-17c73900-6004-11eb-9768-769b207d726a.png">

그 다음으론 State를 바인딩 해주겠습니다.

Mutation인 setRepos로 받은 repos를 tableView에 바인딩 해주는 모습을 볼 수 있습니다.



#### 3. View에 Reactor 넣어주기

Reactor와 View 간의 흐름을 연결해주는 것만 남았습니다. bind()는  View의 Reactor에 새로운 값이 들어왔을 때만 호출되기 때문입니다.



#### ! 전체코드

https://github.com/ReactorKit/ReactorKit/tree/master/Examples/GitHubSearch



### 끝으로

말로만 듣던,, ReactorKit,, 간단하게 예제를 통해 알아보았는데 아무래도 아직 미숙한 점이 많아 틀린 부분이 있을 수도 있습니다. 틀린 부분이 있으면 프로필에 기재된 이메일로 피드백 주시면 감사하겠습니다 :) MVVM으로 프로젝트를 몇개 진행했었는데 

바인딩 방법의 통일을 주니 협업시에도 유용하게 쓰일 것 같아 기대가 됩니다! 더 공부해서 프로젝트에 도입해야겠어요..!





#### 출처

https://medium.com/styleshare/reactorkit-시작하기-c7b52fbb131a

https://eunjin3786.tistory.com/100

