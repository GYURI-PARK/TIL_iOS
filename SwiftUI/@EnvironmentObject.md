## ✨ @EnvironmentObject에 대하여
> Data Essentials in SwiftUI (WWDC20) 영상을 보고 정리한 내용이다. </br>

</br>

### 💡 @EnvironmentObject란?

`EnvironmentObject`은 **뷰 모디파이어(view modifier)** 와 **프로퍼티 래퍼(property wrapper)** 두 가지 역할을 모두 수행한다. </br>

<img width="70%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/6a05a01e-6bb5-4702-ae85-0a81d9f7144c">

>  You use the view modifier in a parent view where you want to inject an ObservableObject. </br>

ObservableObject를 주입하고 싶은 부모 뷰에 `view modifier`로 사용한다. 

</br>
</br>

<img width="70%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/53ceecba-7781-4eb8-9cca-e876396cfdb6">

> And you use the property wrapper in all the views where you want to read an instance of that specific ObservableObject. </br>

`프로퍼티 래퍼`는 해당 ObservableObject의 인스턴스를 읽고자 하는 모든 뷰에서 사용된다. </br></br>

<img width="70%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/9956c1f7-213d-464a-8cb0-295fc8b27b98">

</br>
</br>

따라서 EnvironmentObject로 선언된 값이 필요한 곳에는 자동으로 값을 전달하고, 값이 읽히는 시점에만 해당 값을 추적하여 필요한 업데이트를 수행하게 된다.

</br>
</br>

### 💡 언제 필요할까?

<img width="70%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/4e531d76-e146-4dd2-80a8-31381aae791e">

</br>
</br>

보통의 뷰 계층구조는 위와 같다. </br>
SwiftUI에서 뷰는 매우 경제적이므로 뷰를 작게 만들어 코드의 가독성을 높이고 재사용하기 편하도록 하는 것이 유용하기 때문에 위의 그림처럼 깊고 넓은 뷰 계층 구조를 가지게 된다. </br>
데이터 흐름의 관점에서 살펴보면, 뷰 계층 구조 상단에 `ObservableObject`를 생성하고 여러 뷰에 동일한 인스턴스를 전달하게 된다. </br>

</br>

<img width="70%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/7d1a163d-2b5c-4f8f-8b35-e0738cdadcd2">

</br>
</br>

위의 그림처럼 ObservableObject를 먼 하위 뷰에서 사용해야 할 때가 있다.</br>
하지만 특정 하위 뷰에서 필요한 데이터를 쓰지 않는 다른 뷰를 통해 전달하는 것은 매우 비효율적이므로, 이럴 땐 `EnvironmentObject`를 사용하는 것이 효율적이다. </br>



