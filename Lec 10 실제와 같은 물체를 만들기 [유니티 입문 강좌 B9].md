https://www.youtube.com/watch?v=kYmYCMAiOUk&list=PLO-mt5Iu5TeYI4dbYwWP8JqZMC9iuUIW2&index=10



현실 세계와 같은 실제 물체 만들기



이번시간 프로그래밍 불필요



Plane, Sphere로 물체 만들기



구 누르고 Inspector에서 Add Component 보이죠?

유니티는 컴포넌트 기반 게임 엔진



## 1. 중력 적용하기

Add component - rigidbody

### RididBody

물리효과를 받기 위한 컴포넌트

오.. 마치 중력의 영향을 받아서, 떨어지는 것처럼 보인다.

올려도 떨어진다.



**현실 세계와 같이 중력의 영향을 받는다.**





## 2. 충돌 영역 정하기

collider 뺀다. (줄 그어주는 초록색) 체크 해제로

와 이렇게 되면 저 아래까지 떨어진다.



### Collider

물리 효과를 받기 위한 컴포넌트



collider 복구

radius, 반지름 크게 하면?

초록 선 커짐



**충돌 기준은 보이는 것이 아닌 Collider에 따른다.**

공 두 개 충돌

굴러서 떨어진다.





### RigidBody 설정

mass 1000으로 하면 결과 다르죠?

### RigidBody -> Mass

수치가 높을수록 충돌이 무거워짐.



### RigidBody > Use Gravity

중력 받는지를 결정.



### RigidBody -> Is Kinematic

[Kinematic 운동학적, 동적]

외부 물리 효과를 무시

Is Kinematic으로 움직이는 함정을 만들 때 유용하다.





## 4. 재질 만들기



### Material

오브젝트의 표면 재질을 결정하는 컴포넌트

Default-Material은 못바꿔.

새로 만들어야 한다.



Project 빈 곳 - 우클릭 - Create - Material



**Albedo**로 color 설정

원하는 물체 클릭한 뒤에, InSpector에 끌어서 놓는다.

-> 적용된다.



재질 편집은 새로 생성해서 적용해야 가능.



**Metalic**

금속 재질 수치

0 ~ 1



**Smoothness**

빛 반사 수치

0 ~ 1



그림 공에 넣기도 가능

그림 쭉 끌어와서 Project에 놓는다.

그림을 Albedo 옆에 조그만 네모에 Drag & Drop

ㅋㅋㅋ

**Texture**

재질에 들어가는 이미지

Texture 그려서 집어넣어도 된다.



**Tiling**

텍스쳐 반복 타일 개수

소수점 하면 되긴 되지만 텍스쳐가 잘려서 들어간다.



**Emission**

텍스쳐 발광(밝기) 조절

빛이 물리적으로 나오는 건 아니다.

실질적으로 빛을 내는 건 light라고 따로 있다.





## 5. 물리 재질 만들기

Project 빈 공간 - 우클릭 - Create - Physic Material

**Physic Material**

탄성과 마찰을 다루는 물리적인 재질

마찬가지로 Drag & Drop해서 넣으면 안 보인다.

Sphere Collider - Material 안에 들어간다.



옵션 몇 가지 분석

**Bounciness**

탄성력, 높을수록 많이 튀어오름.

최댓값 1. 더 큰 값 줘도 1로 돌아온다.



**Bounciness Combine**

다음 탄성을 계산하는 방식

Average -> Maximum

Bounciness 1로 놓고 Maximum 하면 무한히 튄다.



**Friction**

마찰력, 낮을수록 많이 미끄러짐



Static Friction - 정지했을 때 마찰력

Dynamic Friction - 움직일 때 마찰력



물리에서 초창기에 움직일 때 필요한 힘의 크기는 이미 움직일 때보다 많이 필요하다.

마찰력 크면 탁 막힘

마찰력 작으면 슥 밀림

마찰력 0 0이면(없으면) , Friction Combine Minimum으로 하면

슥 밀린다.



**Friction Combine**

다음 마찰력을 계산하는 방식.



만화스러운 느낌 내기 위해서

Friction 합산은 최소로, Bounciness 합산은 최대로



Inspector에 보이는 하나하나 컴포넌트라고 부른다.

필수 아이템, 컴포넌트 알아보았다.

현실 세계처럼 물체를 만들기 위한 필수 4대 요소



### 물체 필수 요소

Mesh[점, 폴리곤, UV를 관리하는 구조체], Material[재질], Collider[물리 효과를 받기 위한 컴포넌트], RigidBody[물리 효과를 받기 위한 컴포넌트]

물리 Material까지 추가.. 마찰같은거..


