## ✨ @ObservedObject, @StateObject 정확히 구분해서 사용하기 !

`@StateObject`와 `@ObservedObject` 모두 관찰 중인 객체의 변경에 반응해서 화면을 업데이트할 수 있게 해주는 **프로퍼티 래퍼**입니다. 사실 대부분의 경우 @ObservedObject를 사용해왔어서 그동안 둘의 차이점을 구분할 필요성에 대해 깨닫지 못했습니다.
</br>
</br>

## @ObservedObject란?

<img width="704" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/033eef65-92a3-490f-a74a-db1b6b7199be">

> @ObservedObject 래퍼는 SwiftUI 뷰와 ObservableObject 객체 간의 연결을 설정하는 데 사용됩니다. </br>

ObservableObject 객체가 변경될 때마다 뷰는 알림을 받고 업데이트된 값을 사용하여 다시 렌더링됩니다. 이러한 래퍼는 뷰와 관찰 이러한 래퍼는 뷰와 관찰 대상 객체 간에 바인딩을 생성하여, 관찰 대상 객체가 변경되면 해당 변경 사항이 자동으로 뷰에 업데이트되도록 합니다.
</br>
</br>

- 예시 코드

```swift
final class CounterViewModel: ObservableObject {
  @Published var count = 0

  func incrementCounter() {
    count += 1
  }
}

struct CounterView: View {
  @ObservedObject var viewModel = CounterViewModel()

  var body: some View {
    VStack {
      Text("Count is: \(viewModel.count)")
      Button("Increment Counter") {
        viewModel.incrementCounter()
        }
      }
  }
}
```
</br>

`CounterViewModel`이 `ObservableObject` 프로토콜을 따르기 때문에 뷰 모델을 `@ObservedObject`로 정의할 수 있습니다.
뷰 모델 내부의 @Published로 정의되어 있는 count 값은 버튼을 누를 때마다 증가하게 되고, 이러한 변화를 기반으로 `CounterView`에서는 화면을 다시 그리게 됩니다.

</br>
</br>

## @StateObject란?

앞서 나온 코드를 @ObservedObject 대신 @StateObject를 사용해도 문제가 발생하지 않습니다 ! </br>

```swift
struct CounterView: View {
  @StateObject var viewModel = CounterViewModel()

  var body: some View {
    VStack {
      Text("Count is: \(viewModel.count)")
      Button("Increment Counter") {
        viewModel.incrementCounter()
        }
      }
  }
}
```
</br>

그렇다면 이 둘의 명확한 차이점은 무엇일까요? </br>

결론부터 이야기하자면, **@StateObject를 통해서 관찰되고 있는 객체는 그들을 가지고 있는 화면구조가 재생성되어도 파괴되지 않는다 !** 는 것입니다.
이 점은 `화면이 다른 화면을 포함하고 있는 경우`를 생각하면 이해하기 쉽습니다. </br>

@StateObject의 작동방식을 알아보기 위해 기존의 코드를 다른 코드 안에 넣어보겠습니다. </br>

```swift
struct RandomNumberView: View {
  @State var randomNumber = 0

  var body: some View {
      VStack {
        Text("Random number is: \(randomNumber)")
        Button("Randomize number") {
          randomNumber = (0..<1000).randomElement()!
        }
    }.padding(.bottom)

  CounterView()

  }
}
```
![Simulator Screen Recording - iPhone 14 Pro - 2023-09-12 at 23 35 49](https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/1d9cb373-86e0-40d0-9ebe-1cf081c04b8f)

</br>

랜덤 숫자 화면은 Randomize 버튼을 통해 새로운 랜덤 숫자를 생성합니다. 이 때 `randomNumber`에는 `@State` 프로퍼티 래퍼가 붙어있기 때문에 값이 바뀔 때마다 카운터 화면이 새로 그려집니다.
위애서 확인할 수 있듯 새로운 랜덤 숫자가 생성될 때 카운터 숫자는 초기화됩니다.

</br>

다음은 `@StateObject`를 사용했을 때 랜덤 숫자 화면입니다.

![Simulator Screen Recording - iPhone 14 Pro - 2023-09-12 at 23 38 33](https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/79777b0c-528e-4dd8-b157-9bf83474747b)

</br>

이전과 달리 @StateObject로 감싼 뷰모델은 화면이 새로 그려져도 카운터 숫자의 값이 초기화되지 않고 그대로 남아있습니다. 

</br>

## ✨ 결론

SwiftUI가 화면을 만들거나 다시 그릴 수 있는 가능성이 있는 경우엔 내부에 `@ObservedObject`를 쓰는 것은 안전하지 않습니다. `@ObservedObject` 객체를 외부에서 주입하는 것이 아니라면 `@StateObject`를 사용하는 것이 화면이 다시 그려져도 항상 같은 결과를 얻을 수 있을 것입니다.
따라서 `@StateObject`를 사용하면 `@ObservedObject`를 생성하는 화면에서도 일관된 결과를 보장할 수 있습니다.

</br>

ObservedObject
- ObservedObject를 사용하면 해당 객체는 뷰가 처음으로 초기화될 때 생성되어야 합니다.
- 뷰가 처음 초기화될 때 생성되기 때문에 뷰의 수명과 동일한 수명을 가진 객체에 적합합니다.

</br>

StateObject
- WWDC2020에서 공개된 개념으로 SwiftUI가 View를 다시 랜더링할 때 실수로 취소되는 것을 방지해줍니다.
- StateObject를 사용하면 해당 객체는 뷰가 처음 초기화될 때 생성되며, 뷰가 소멸될 때까지 유지됩니다.
- 이는 뷰의 수명과 독립적으로 객체를 유지하고자 할 때 유용합니다. 예를 들어, 뷰가 다시 렌더링되더라도 객체를 다시 만들지 않고 유지하고자 할 때 유용합니다.

</br>

따라서 `StateObject`가 유용한 경우는 객체가 뷰가 초기화될 때 생성되어야 하지만 **뷰가 여러 번 재렌더링**될 때 유지되어야 하는 경우, **객체가 뷰의 수명과 독립적으로 유지**되어야 하는 경우. 라고 할 수 있을 것 같습니다 !

</br>
</br>

[해당 공부 내용을 다음 코드 리뷰에서 활용할 수 있었다😃](https://github.com/DeveloperAcademy-POSTECH/MacC-Team11-UMM/pull/221)

