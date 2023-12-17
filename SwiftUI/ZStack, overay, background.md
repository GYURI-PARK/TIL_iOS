## ✨ ZStack, overlay, background 뭐가 다를까?
> SwiftUI에서 ZStack, overlay, background은 레이아웃 및 뷰의 겹침을 제어하는 데 사용되는 다른 속성들이다. 그렇다면 어떤 차이점이 있는지 알아보자 !

</br>

### 💡 ZStack

- ZStack은 자기 자신 안에 있는 View들을 **독립적으로 취급**한다.
- 따라서, 서로 크기가 다른 View들이 ZStack안에 존재할 때, 그 안의 가장 큰 **width, height에 따라 size가 결정**된다.
- 즉, 하위 뷰의 크기에 따라 ZStack 뷰의 크기도 변하게 된다.

```swift
ZStack {
    // 뒷 배경에 표시될 뷰
    Rectangle().fill(Color.blue)
    
    // 앞에 표시될 뷰
    Text("Hello, SwiftUI!")
}
```

</br>
</br>

### 💡 .overlay

- overlay는 view modifier로서, 상위 뷰에 종속된다.
- 즉, 상위 뷰를 기준으로 하위 뷰를 앞쪽에 얹어줄 때 사용되는 것이다.
- 예를 들어, Text의 overlay라면 Text에 대해 종속되어 좌표가 결정된다.


```swift
Text("Hello, SwiftUI!")
    .overlay(
        Rectangle()
            .stroke(Color.red, lineWidth: 2)
    )
```

</br>
</br>

```swift
VStack {
            
            Rectangle()
                .fill(.yellow)
                .frame(width: 150,height: 150)
                .overlay(Text("bottom TabView"), alignment: .bottom)
                .border(.red).padding(.bottom, 50)
            
            ZStack (alignment: .bottom) {
                Rectangle()
                    .fill(.yellow)
                    .frame(width: 150,height: 150)
                
                Text("bottom TabView")
            }.border(.red)
        }
```

<img width="199" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/82e50ff3-da65-403e-b3fd-22ff2ec9bb41">

위 사각형은 overlay를 사용해 Text를 얹어주었고, 아래의 사각형은 ZStack을 사용해 Text를 나타내주었다. </br>
현재 보이는 결과는 같지만 글자의 크기를 키워보면 둘의 차이를 비교할 수 있다. </br>

<img width="221" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/7d48e8ff-f717-434b-b6ab-0c1f387b0296">

1. 오른쪽 상단의 `overlay`를 사용한 코드의 경우 상위 뷰인 Rectangle View의 크기에 종속되기 때문에 텍스트 사이즈를 키웠을 때 Rectangle 안에서 text size가 커진다. </br>
2. 반면, 반면 하단의 `ZStack`로 구현한 코드는 각 View가 독립적이기 때문에 Text의 크기가 커진 만큼 ZStack의 크기가 커진 것을 볼 수 있다. </br>

</br>

#### 따라서 overlay의 경우 크기나 위치가 고정되어있는 상태에서 상위 뷰를 꾸며주는 역할로 쓰기에 적합하고, ZStack의 경우 직접적으로 다른 뷰와 연관성이 없도록 UI를 구성할 경우에 적합하다.

</br>
</br>


### 💡 .background
> 위에서 언급한 ZStack과 overlay는 위(앞)쪽으로 layer를 쌓아주는 것이었다면, background는 반대로 뒤쪽으로 layer를 겹쳐주는 것이다.

- overlay와 마찬가지로 view modifier로서, 상위 뷰에 종속된다.
- .background 모디파이어는 주어진 뷰에 배경을 추가하는 것이다.
- 그러나 이 모디파이어는 주어진 뷰와 함께 동작하기 때문에 해당 뷰의 일부로 간주된다.

```swift
ZStack {
    Text("Hello, SwiftUI!")
    Rectangle().fill(Color.blue)
}
.background(
    Color.yellow
)
```
> ZStack 전체에 노란색 배경을 추가한다.

<img width="221" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/0ebc22a2-476b-40c2-aff4-653422fa05c0">

</br>

```swift
Text("Hello, SwiftUI!")
    .background(
        Color.yellow
    )
```
> Text 뷰에만 배경이 추가된다.

<img width="221" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/010f9488-0cc6-49c6-9e8e-a8957af76cab">
