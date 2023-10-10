## ✨ Core Data의 기초 이해하기 !

> MacC 프로젝트에서 Core Data와 이번 WWDC23에 새롭게 나온 SwiftData 중 고민하다, 결국 SwiftData도 Core Data가 기본이라는 말에 Core Data를 쓰기로 했다. 
오늘 올리버의 강의를 듣고, 정리를 하며 복습을 해보고자 한다 !

</br>

## 👀 Data Model

Core Data를 사용하기 위해선 Data Model 파일을 추가해줘야 한다. 데이터 모델은 앱의 객체와 그 객체들이 어떻게 서로 관련되어 있는지를 나타내는 객체 그래프에 대한 정보를 담고 있는 것이다.
`.xcdatamodeld` 파일에  `Entity`, `Attributes`, `Relationships`, `Fecthed Properties`를 추가해주게 되는데 이들은 모델 계층의 객체(class)와 프로퍼티라고 볼 수 있다.

### Entity

Attributes, Relationships, Fetched Properties는 모두 Entity의 프로퍼티이다.

#### Attributes
<img width="485" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/5de8019d-cc5f-4f9e-bb01-109ee55c0f72">

- Entity를 구성하는 프로퍼티, 모델 타입의 프로퍼티

#### Relationships
<img width="491" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/2412a686-410f-4405-bf04-320847cca98e">

- Entity들끼리 어떻게 관련되어 있는지, 변경 사항이 Entity 간에 어떻게 전달될지를 정의한다.
- Destination : relation을 갖는 상태 Entity
- Inverse : 역방향의 relationships
- Inverse를 설정해줘야 변화가 생겼을 때 코어 데이터가 양쪽 모두 변화를 전달 할 수 있다.

</br>

## 👀 Core Data Stack

데이터 모델을 만든 뒤에는 앱의 모델 레이어를 협력해서 도와주는 클래스들을 추가해줘야 한다. 이러한 클래스들을 Core Data Stack이라고 한다.

</br>

<img width="688" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/1e66a293-f12a-4ff7-abec-d8a20dfd62e6">

데이터 모델은 위와 같이 구성되어 있다.

### Managed Object Model

`NSManagedObjectModel`
</br>



### Managed Object Context

`NSManagedObjectContext`

### NSPersistentStoreCoordinator



