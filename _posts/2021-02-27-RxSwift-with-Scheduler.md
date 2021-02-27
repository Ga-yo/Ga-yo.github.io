---
title: "RxSwift-Scheduler 알아보기"
date: 2021-02-27 21:31:50 -0400
categories: RxSwift 
toc: true
tags:
 - Scheduler
 - RxSwift
---

### Scheduler?

스케줄러는 스레드의 Reactive적 개념입니다. 요구사항에 맞게 비동기로 동작할 수 있도록 바꿔줘야하는데 이때 스케줄러를 사용합니다. 한 마디로 작업이 실행되는 스레드를 정해주는 클래스라고 볼 수 있습니다.

- 스케줄러는 Rx 코드를 어느 스레드에서 실행할지 지정할 수 있습니다.
- Cocoa에선 멀티 스레딩 처리를 위해서 GCD를 사용하지만 RxSwift는 Scheduler로 이용합니다.
- 스케줄러를 별도로 지정하지 않으면 main 스테르에서 동작을 실행합니다.



```swift
let mainScheduler = MainScheduler.instance
let concurrentScheduler = ConcurrentDispatchQueueScheduler.init(qos: .background)
```



### Scheduler 연산자

#### observeOn()

작업 수행을 다른 스케줄러에서 하고 싶을 때

```swift
sequence1.observeOn(backgroundScheduler)
	.map { n in
    print("이건 백그라운드 스케줄러에서 실행될 것입니다.")
	}.observeOn(MainScheduler.instance)
	.map { n in
  	print("이건 메인 스케줄러에서 실행될 것입니다.")
  }
```

- 여러 번 호출할 수 있습니다
- 동작하는 스레드를 바꿀 수 있습니다.



#### subscribeOn()

sequence 생성과 dispose call을 특정 스케줄러에서 하고 싶을 때, Observable의 프로세싱이 어디서 일어나게 할 것인지 결정하는 연산자입니다.

```swift
let fruit = Observable<String>.create { observer in
    observer.onNext("[apple]")
    sleep(2)
    observer.onNext("[pineapple]")
    sleep(2)
    observer.onNext("[strawberry]")
    return Disposables.create()
}

let globalScheduler = ConcurrentDispatchQueueScheduler(queue: DispatchQueue.global())
fruit
    .subscribeOn(globalScheduler)
    .subscribe(
        onNext: {print($0)}
)
    .disposed(by: db)
```

- observeOn() 함수는 처리된 결과를 구독자에게 전달하는 스레드를 지정합니다.
- 처음 지정한 스레드를 고정시키므로 다시 호출해도 무시합니다.



### Scheduler의 종류

>  Serial
>
> - Rx가 직렬적으로 진행됩니다
>
> Concurrent
>
> - Rx가 동시에 진행됩니다



#### Main

메인 스레드에서 가장 위에 존재하는 것으로 UI를 갱신하려면 이 스케줄러로 변경하여 사용됩니다.

API request같은 heavy task는 피해야합니다.

#### Current

현재 스레드를 이용합니다.

#### SerialDispatchQueue

background에서 추출하는 일을 처리할 때 사용됩니다

Ex) firebase를 사용할 때 서버의 endpoint에 너무 많은 pressured를 줄일 수 있습니다.

#### ConcurrentQueue

SerialDispatchQueue와 같이 추출하는 일을 수행할 때 처리됩니다. 하지만 병렬적!!

#### Test

말그대로 테스트를 위한 것이며 production code에는 사용하지 않습니다.

