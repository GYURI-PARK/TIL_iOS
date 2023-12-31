## ✨ 싱글톤 패턴에 대해 알아보기 !
> 왜 ! Why ! 싱글톤을 사용하게 되었냐면 ?! `노마드월렛`개발에 한창이다.
> 우리는 (1차 MVP인 지금은 28개의 국가만을 제공하지만) 결국 몇 백 개의 국가 에셋이 필요할 것이고 이 많은 데이터를 어떻게 효율적으로 관리하고 편리하게 쓸 수 있을지 팀내 개발자들과 많은 논의를 했었고, 결과적으로
> 전체 데이터 관리는 **csv파일**을 사용하여 개발자뿐만 아니라 PM, 디자이너도 데이터를 쉽게 추가하거나 관리할 수 있도록 하기로 했다. 그 다음 불러온 데이터를 관리할 모델 레이어가 필요했는데, 이 때 **싱글톤 패턴**을 사용하기로 하였다 !

</br>
</br>

## 싱글톤 패턴(Singleton Pattern)이란?

> 특정 용도로 객체의 인스턴스를 단 한 개만 생성하여, 공용으로 사용하고 싶을 때 사용하는 디자인 유형 </br>

즉, 클래스에 대한 인스턴스는 최초 생성 시에 딱 한번만 생성해 전역에 두고, 이렇게 만들어진 인스턴스를 어떤 클래스에서든 접근 가능하게 하는 것이다. </br>

### 장점

1. 메모리 측면

최초 한번의 연산자를 통해 고정된 메모리 영역을 사용하기 때문에 추후 해당 객체에 접근할 때 메모리 낭비를 방지할 수 있다. </br>

<img width="397" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/9f377063-aeb8-4b15-a3cc-25bc9abbdfc0">

</br>
</br>

예를 들어, 위 형태의 csv 파일에서 인덱스에 맞는 한글 국가명, 장소명 등을 불러오기 위해 열에 해당하는 변수들을 가지고 있는 하나의 클래스를 선언할 수 있을 것이다.

<img width="312" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/eb484a4f-439f-4ebc-99bd-06c10a53c3ee">

> 📝 프로젝트 회고 </br>
메모리 측면에서 보자면, 싱글톤을 도입하기 전 국가에 맞는 String값을 부르기 위해 [Int: String] 형태의 딕셔너리를 반환하는 함수를 만들어 사용했었다.
> 매번 해당 String이 필요할 때마다 함수를 사용해 딕셔너리를 만들어주는 것이 비효율적이라는 생각이 들어 만들어진 딕셔너리를 유저 디폴트에 저장해서 최초 뷰 생성 시에만 딕셔너리를 생성하고,
> 이후에는 유저 디폴트에서 값을 가져와 사용하는 방법을 생각했었지만, 전체 국가를 관리해주는 모델 레이어가 없다는 점과 하나의 국가에 대해 딕셔너리를 여러 개 만들어줘야한다는 문제점 때문에 싱글톤을 도입했었다.

</br>
</br>

2. 데이터 공유 이점

다른 클래스 간에 데이터 공유가 쉽다는 점이 우리가 싱글톤 패턴을 선택한 가장 큰 이유이다. </br>
싱글톤 인스턴스가 전역으로 사용되는 인스턴스이기 때문에 다른 클래스의 인스턴스들이 접근하여 사용할 수 있다. </br>


</br>

## Swift에서 싱글톤 패턴 사용하기
### 1. static 프로퍼티로 인스턴스 생성하기

```swift
class UserInfo {
  static let shared = UserInfo()

  var id: String?
  var name: String?
  var age: Int?
}
```
</br>

> static을 사용해 인스턴스를 저장할 **프로퍼티**를 생성해준다.
> 이 프로퍼티는 전역으로 저장될 것이기 때문에, 다른 class들에서 접근이 가능하다.

</br>

### 2. init()을 private으로 지정하기

```swift
class UserInfo {
  static let shared = UserInfo()

  var id: String?
  var name: String?
  var age: Int?

  private init() { }
}
```
</br>

> 다른 곳에서 init()함수가 호출되어 인스턴스가 생성되는 것을 방지하기위해 private으로 지정해준다.

</br>

### 3. 싱글톤 클래스에 접근하기

```swift
let userInfo = UserInfo.shared
userInfo.id = "Doris"
```

</br>

> 어느 클래스에서든 `shared` 프로퍼티에 접근하면 하나의 인스턴스를 공유할 수 있다.


