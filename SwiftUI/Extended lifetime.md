## ✨ @SceneStorage와 AppStorage
> 데이터의 수명관리는 매우 중요하다. State, StateObject, Constants로 정의된 데이터들의 경우 앱의 프로세스 수명과 연결되어 있어서,
> 앱이 종료되고, 다시 시작될 경우에 그 값이 복원되지 않는다. 따라서 lifetime의 확장 개념으로 데이터를 저장 및 복원해줄수 있는 것이 필요하며
> 그 때 사용되는 것이 이 두 가지이다 ! ! ! </br>
> ❗️ 데이터 모델이 아닌, 데이터 모델과 함께 사용할 수 있는 가벼운 저장소 개념이다. ❗️

</br>
</br>

### 💡 @SceneStorage

<img width="50%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/4fc2069d-c7b0-44a1-8f42-98c5d18d7123">

</br>
</br>

- **Scene 단위**로 범위가 지정된 프로퍼티 래퍼
- 뷰 내부에서만 접근이 가능하며, 앱의 현재 상태에 대한 가벼운 정보를 저장할 때 유용

</br>

```swift
@SceneStorage("selection") var selection: String?
```

위와 같은 형태의 코드로 저장할 데이터를 정의할 수 있으며, State처럼 동작하기 때문에 새로운 source of truth가 선언되는 것이다. </br>
그리고 이 때의 데이터 원천은 Scene 전체의 source of truth이므로, 기기가 재시작될 때에도 데이터가 유지된다.

</br>
</br>
 
간단한 코드를 통해 @SceneStorage와 @State의 차이점을 비교해볼 수 있다. </br>

```swift
struct SceneStorage: View {
    
    @SceneStorage("textInput") var storageText = ""
    @State var stateText = ""
    
    var body: some View {
        VStack(spacing: 20) {
            
            // @SceneStorage 사용
            Text("1. @SceneStorage 사용")
                .font(.title)
            
            TextEditor(text: $storageText)
                .border(Color.yellow)
                .padding(.horizontal)
            
            // @State 사용
            Text("2. @State 사용")
                .font(.title)
            
            TextEditor(text: $stateText)
                .border(Color.yellow)
                .padding(.horizontal)
        }
    }
}
```
> 상단의 TextEditor에는 `SceneStorage`로 정의된 text를 사용하였고, 하단의 TextEditor에서는 `State`를 사용하였다. </br>

이제 두개의 결과를 비교해보자. </br>

</br>

![Simulator Screen Recording - iPhone 15 Pro - 2023-12-19 at 14 39 17](https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/88ec903c-fc8c-4a06-8fcd-8a8901c68835)

</br>
</br>

`State`로 정의된 텍스트는 뷰를 나갔다가 들어왔을 때, 사라져있지만, `SceneStorage`로 정의된 텍스트는 그대로 남아있는 것을 볼 수 있다.

</br>
</br>

### 💡 AppStorage

<img width="50%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/dff85512-2c0f-44d3-ab35-f4a4d9aab1c2">

</br>
</br>

- **앱 범위**의 글로벌 저장소로, `UserDefaults`를 사용하여 영속적으로 저장
- 어디서든 사용할 수 있으므로, 앱 내부나 뷰 내부에서 모두 접근 가능
- `UserDefaults`와 마찬가지로, 설정과 같은 작은 데이터를 저장할 때 유용

</br>

```swift
@AppStorage("syncProgress") private var syncProgress = true
```

SettingView에서 위와 같이 AppStorage 프로퍼티 래퍼를 정의하고 키를 지정하면 된다. </br>
SceneStorage와 마찬가지로 새로운 source of truth가 만들어지고, 앱 전체에 적용된다. </br>


</br>

<img width="50%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/6043b985-c9d4-4284-9d9e-b3fbd1aeb043">

</br>
</br>

위에서도 언급했지만, 이 둘은 매우 작은 저장소로 쓰이며, 저장했을 때 꼭 필요한 정보만 저장해야 한다.
