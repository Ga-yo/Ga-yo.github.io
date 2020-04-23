### View

#### 뷰의 초기화

```swift
init(frame: CGRect) //view가 코드로 만들어졌을 때

init(coder: NSCoder) //storyboard에서 만들어진 경우	
```

__둘 다 구현해야함__

```swift
override func awakeFromNib(){
//스토리보드로 부터 UIView가 만들어졌을 때 호출
}
```

#### 어떻게 드로잉?

그 전에 - -

1. CGFloat : 
2. CGPoint : 
3. CGSize :
4. CGRect :

부동 소수점 숫자를 사용하는 것에 대해 - -

1. 원점이 좌측 상단에
2. 단위는 points (!=픽셀)

