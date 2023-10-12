## ✨ @GestrueState를 이용해 Long Press Gesture를 사용해보자 !

> STT 기능 개발 중 녹음버튼을 누르고 있는 동안만 녹음이 진행되도록 해야하기 때문에 Long Press와 관련된 개념을 공부하다가 처음 보는 프로퍼티 래퍼가 등장해 정리해보려고 한다. </br>

</br>

## @GestureState

- 프로퍼티 래퍼의 일종으로 사용자가 제스터를 수행하는 동안 속성을 업데이트하고 제스처가 끝날 때 속성을 초기 상태로 설정하는 역할을 한다.

</br>

## 사용법

사용법은 간단하다. 

1. 속성을 @GestureState로 선언한다.
2. 선언된 속성을 원하는 제스처의 updating 매서드의 매개변수로서 바인딩을 통해 전달하고 업데이트를 받으면 된다.

요약하면 @GestureState 속성을 선언하면 해당 제스처와 관련된 상태를 추적할 수 있고, 이것은 제스처가 **활성 상태**일 때 제스처에 의해 **업데이트**되며, 제스처가 **비활성 상태**가 되면 자동으로 **초기 상태**로 재설정된다. 
따라서 일시적인 상태나 제스처에 의한 변경을 추적하고 효과적으로 처리할 때 유용하다.

</br>

## 예제코드

```swift
struct SimpleLongPressGestureView: View {
    
    // 1. 제스처 상태를 나타내는 변수 선언
    @GestureState private var isDetectingLongPress = false


   // 2. 제스처 정의
    var longPress: some Gesture {
	// 2-1. 최소 지속시간 3초 지정
        LongPressGesture(minimumDuration: 3)
	    // 2-2. updating 매서드를 통해 LongPressGesture가 업데이트될 때 수행되는 작업 정의
            .updating($isDetectingLongPress) { currentState, gestureState, transaction in
                gestureState = currentState
            }
	    // 2-3. currentState는 현재 제스처의 상태를 나타내며 gestureState에 할당하여 이 상태를 업데이트한다.
	
    }


    var body: some View {
        Circle()
            .fill(self.isDetectingLongPress ? Color.red : Color.green)
            .frame(width: 100, height: 100, alignment: .center)
            .gesture(longPress)
    }
}
```

</br>

### 결과화면
![Simulator Screen Recording - iPhone 15 Pro - 2023-10-12 at 23 58 28](https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/2ea08338-400d-4a79-8d50-8998feb464de)

</br>

3초 미만의 클릭 시에는 빨간색으로 색이 바뀌게 되고, 
누르고 있는 동안에도 빨간색을 유지하다가,
최소 시간은 3초를 넘기게 되면 다시 초록색으로 변하게 된다. 

</br>

추가로 녹음 기능의 경우 시간이 정해져 있는 것이 아니라 사용자가 원할 때 손을 떼기 때문에 `minimumDuration`을 `.infinity`로 설정해주면 된다.


