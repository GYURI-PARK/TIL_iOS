## ✨ @State과 @Binding을 통해 SwiftUI의 데이터 흐름 이해하기 !

> SwiftUI로 처음 개발을 하면서 가장 힘들었던 부분은 데이터의 흐름을 이해하는 것이었다. 그러면서 왜 이렇게 Property Wrapper는 많은 걸까! 라는 생각도 했었다. 대부분 처음 시작하는 사람들이 겪었을 문제라고 생각한다. 
그래서 매번 대충 감으로 이해한 듯한 데이터 흐름에 대해 완벽! 까지는 아니더라도 정리를 통해 흐름을 이해해 보려고 한다. 
</br>

## Property Wrapper 찍먹

우선 SwiftUI의 특징 하나를 소개하고자 한다. </br>
SwiftUI의 특징이자 강력한 장점 중 하나는 **데이터가 변경되었을 때** (= 상위 계층의 뷰에서 변경된 데이터 or 하위 계층에서 `사용자의 응답`에 따른 데이터 변경), </br>
이것이 외부 이벤트로 인한 것인지 사용자가 수행한 동작으로 인한 것인지에 관계 없이 **인터페이스의 영향을 받는 부분을 자동으로 업데이트**해 준다는 것이다. </br>
즉 UIKit의 뷰 컨트롤러에서 처리하던 대부분의 작업을 자동으로 수행해 준다는 것이다. </br>

위의 특징을 이해하고 나면 SwiftUI에서 왜 그렇게 많은 Property Wrapper가 필요하고 이것들이 얼마나 유용한 것인지 조금은 이해할 수 있을 것이다. 
> SwiftUI에서 데이터와 UI의 연결은 매우 중요한데 SwiftUI에서는 이러한 데이터 흐름에 대한 여러 상태에 대응하기 위해 `프로퍼티 래퍼`를 사용한다.

</br>
</br>

## 한 컷 정리
<img width="845" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/fd77ce68-390e-4c71-ac45-b1d02ec02c2c"> </br>
> 출처 : objc.io의 Chris Eidhof 이미지

</br>
</br>

## @State

일반적으로 struct는 값 타입이여서 struct내의 값을 변경할 수 없다.
따라서 SwiftUI에서는 `@State`를 제공해 struct내의 값을 변경할 수 있게 해준다. 

```swift
struct ContentView: View {
  @State private var number = 0
}
```
- 일반적으로 @State 변수는 private으로 선언되고, 다른 view와 공유되지 않는다.
- @State로 선언된 변수는 view의 소멸, 재생성과는 무관하게 지속적으로 변형 가능하다.

정리하면 </br>
- SwiftUI에서는 @State로 선언된 프로퍼티의 저장소를 관리
- @State를 이용하면, 로컬에서 잠시 값을 저장하고 있다가, 해당 데이터의 변화에 맞춰 저장된 값들의 UI를 모두 다시 랜더링

 </br> 

### A single source of true
> Use state as the single source of truth for a given value stored in a view hierarchy.

뷰에서 사용하는 데이터는 하나의 원천을 가진다. 따라서 view hierarchy 중 여러 view에서 state로 선언된 프로퍼티들이 쓰이더라도, 데이터의 원천은 하나이다. </br>

만약 부모 뷰가 자식 뷰에게 @State 프로퍼티를 전달하면, SwiftUI는 부모 뷰의 @State 프로퍼티 값이 바뀔 때마다 자식 뷰를 업데이트하겠지만, **자식 뷰는 그 값을 수정할 수는 없다.** 그래서 아래에 나오는 `@Binding`이 필요하다.
자식 뷰가 값을 변경하도록 하기 위해서는, `@Binding`을 전달해주어야 한다.

</br>

WWDC20 영상에 나온 말의 일부를 인용하자면, **The simplest source of truth in SwiftUI is State.** </br>
이는 SwiftUI에서 데이터나 상태를 나타내는 가장 기본적이고 간단한 방법은 `@State` 프로퍼티 래퍼를 사용한다는 것이다.

</br>
</br>

## @Binding

@Binding은 부모 view의 @State와 같은 값을 양방향으로 연결되도록 해준다. </br>
위에서 설명한 `@State`로 선언된 데이터가 **원천 데이터**라면 `@Binding`으로 선언된 변수는 **데이터에 따라 함께 반응하는 데이터**라고 할 수 있다.

</br>

```swift
struct PlayerView: View {
	@State private var isPlaying: Bool = false
    
    var body: some View {
    	PlayButton(isPlaying: $isPlaying) // 자식 뷰에게 binding 전달
    }
}

struct PlayButton: View {
	@Binding var isPlaying: Bool // 부모 뷰에게서 binding 값 받음
    
    var body: some View {
    	Button(isPlaying ? "Pause" : "Play") {
        	isPlaying.toggle() // 부모 뷰에게서 받은 값 수정
        }
    }
}
```
> 위의 코드에서 부모 뷰(PlayerView)에서 isPlaying변수를 @State로 선언해 주고, 자식 뷰(PlayButton)에서는 뷰모 뷰로부터 값을 받기 위해 @Binding을 사용해 선언해준다. </br>
> 부모 뷰에서 받은 Binding 값은 이제 자식 뷰에서 수정이 가능하다.

</br>
</br>


*참고자료* </br>
[WWDC19](https://developer.apple.com/videos/play/wwdc2019/226/) ,
[WWDC20](https://developer.apple.com/videos/play/wwdc2020/10040/)
