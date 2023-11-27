## ✨ 기존 iOS 프로젝트에 watchOS를 추가해보자 !
> 노마드월렛의 2차 mvp는 watchOS를 사용해 배낭 여행자들이 애플워치를 사용해 소비기록을 더욱더 쉽게 할 수 있도록 하는 것이다 ! 5일도 채 남지 않은 시간이지만... 일단 공부해 보자 !

</br>
</br>


## 기존의 iOS 프로젝트에 watchOS 앱 타겟 추가하기
> 아래의 내용은 [Apple 공식문서](https://developer.apple.com/documentation/watchkit/setting_up_a_watchos_project)를 보고 작성하였다.

</br>

순서는 다음과 같다.
1. Xcode에서 iOS 앱 프로젝트를 열기
2. 파일(File) > 새로 만들기(New) > 타겟(Target) 선택
3. watchOS 탭 선택
4. `Watch App for iOS App`을 선택하고 다음(Next)을 클릭
5. 타겟 옵션 창에서 프로젝트의 제품 이름(Product Name)을 입력. 알림 또는 복잡한 기능을 구현할 계획이라면 해당 확인란을 선택하고 완료(Finish)를 클릭

   <img width="50%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/1683c065-5695-4aac-b5a9-d63d2ad2d36d">
   </br>
   
6. 그런 다음 Xcode에서 새로운 스킴을 워치 타겟에 대해 활성화할 것인지 물으면 활성화(Activate)를 클릭
   
   <img width="40%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/55b1f8e9-b748-4dfc-abf0-d6d1edff2b13">

</br>

위 과정을 거치고 나면, 아래와 같이 수많은 폴더들 사이로 Watch App이 생성된 것을 볼 수 있다.

<img width="290" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/78cc5c02-356a-471a-9dab-69fef5f343ea">

</br>

아래와 같이 스키마를 선택할 수도 있다.

<img width="246" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/04e08762-17ba-4af3-af8d-8a97fd6f951d">

</br>
</br>

7. (Optional) `General > Deployment Info > Supports Running Without iOS App Installation` </br>
   해당 박스를 체크할 경우 iOS App 없이도 실행을 지원해준다.

<img width="728" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/3a1cd282-e5be-4327-9190-dd0551805df3">


</br>
</br>

## iOS 데이터 watchOS에서 사용하기
> SwiftUI로 작성한 코드의 경우 데이터 모델, 리소스 파일의 수정없이 모든 뷰를 watch App에서 재사용할 수 있다.

</br>
</br>

우선 자동으로 생성된 WatchApp파일을 삭제하고 iOS 파일을 이용한다.

<img width="292" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/406a0cbc-a282-45cf-b13d-5d19ee466379">

</br>
</br>

그런 다음 사용하는 파일들 전부에다 Target을 추가해준다.

처음엔 필요한 파일들만 target에 추가할 생각이었지만, 그냥 모든 파일을 추가하기로 했다.

</br>

<img width="60%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/9ae1dd1d-7d03-4ae7-8bfe-1396a2d18f4f">

</br>

</br>

앱 아이콘도 추가해준다.

</br>

<img width="50%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/fae3211d-7d87-4f26-b89f-674f5651ddc8">

<img width="717" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/e22ce488-a23e-4352-acd6-c4953f7aca76">

