https://www.youtube.com/watch?v=pTc1dakebow&list=PLO-mt5Iu5TeYI4dbYwWP8JqZMC9iuUIW2&index=14



## 목차

- [1.계획하기](#1.계획하기)
- [2.플레이어](#2.플레이어)
- [3.아이템](#3.아이템)
- [4.카메라](#4.카메라)
- [5.결승점](#5.결승점)
- [6.장면이동](#6.장면이동)
- [7.스테이지](#7.스테이지)
- [8.UI](#8.UI)
- [Build](#Build)



예제 모아서 게임 만들기



게임 개발은 계획부터 차근차근!





## 1.계획하기

1. 게임 이름: 굴러서 템먹기
2. 장르: 캐주얼 액션
3. 목표: 지형을 뛰어넘어 굴러서 템을 먹고 목표지점에 도달.
4. 구성: 공(Player), 템(Item), 지형(Platform), 결승점(Point)
5. 구상도: 생략





## 2.플레이어

Hierarchy - 우클릭 - 3D Object - Sphere



```c#
public class PlayerBall : MonoBehaviour
{

    Rigidbody rigid;

    void Awake()
	{
        rigid = GetComponent<Rigidbody>();
	}

    void FixedUpdate()
    {
        float h = Input.GetAxisRaw("Horizontal");
        float v = Input.GetAxisRaw("Vertical");

        rigid.AddForce(new Vector3(h, 0, v), ForceMode.Impulse);
    }
}
```



와우 막 움직인다~



근데 너무 빨라

=> Mass를 크게 해도 되고, Power를 낮춰도 된다.



이제 점프!

Update에서 받자

```c#
	public float jumpPower = 10;

    void Update()
	{
        if(Input.GetButtonDown("Jump"))
		{
            rigid.AddForce(new Vector3(0, jumpPower, 0), ForceMode.Impulse);
        }
	}
```

jumpPower를 public하면 unity 안의 Player Ball에서 자유롭게 컨트롤할 수 있다.

[아예 public float jumpPower; 일케 하심]



플레이어 움직임 구현 완료



Create Material해서 재질 설정도 가능.



무한 점프 방지

한 번만 뛰게

물리충돌이벤트를 쓰자.



```c#
	bool isJump;

    void Update()
	{
        if(Input.GetButtonDown("Jump") && !isJump)
		{
            isJump = true;
            rigid.AddForce(new Vector3(0, jumpPower, 0), ForceMode.Impulse);
        }
	}
    
    void OnCollisionEnter(Collision collision)
	{
        if(collision.gameObject.name == "Floor")
		{
            isJump = false;
		}
	}
```





## 3.아이템

원하는 형태 쓰십셔

저는 Capsule 쓰겠다.



아이템 회전시키고 싶다.

회전 -> **transform**에서 한다.

transform은 변수가 따로 있다.



Rotate(Vector3)

매개변수 기준으로 회전시키는 함수



```c#
public class ItemCan : MonoBehaviour
{
    public float rotateSpeed;
    void Update()
    {
        // transform.Rotate(new Vector3(0, 1, 0));
        transform.Rotate(Vector3.up * rotateSpeed * Time.deltaTime);
    }
}
```

**Update**에서 움직인다 -> **Time.deltaTime**

어떤 환경이든 항상 움직이는 값은 동일하기 때문에 넣어준다.



Unity 가서 rotateSpeed값 조절해주면 된다.



-> 돌긴 도는데 y값과 평행하게 돌아서 안 도는 것처럼 보인다.

-> 제가 원하는 건 게임 세계의 y축과 평행하게 도는 것.

Unity 상에서 왼쪽 위에 있는 Local 버튼을 클릭하면 Global로 바뀌는 것을 확인할 수 있다.



**transform.Rotate** 오버로드 한다.

오버로드: 이름은 같지만 매개변수가 다른 함수를 호출

```c#
    public float rotateSpeed;
    void Update()
    {
        // transform.Rotate(new Vector3(0, 1, 0));
        transform.Rotate(Vector3.up * rotateSpeed * Time.deltaTime, Space.World);
    }
```

뒤에 **Space.world** 추가



도는 것 확인 가능.

좀 느리면 rotateSpeed를 올린다.



이제 player가 아이템과 부딪히면, 아이템을 먹을 수 있어야겠죠?

물리적 충돌 필요없기 때문에 is Trigger 체크



점수 획득 - player쪽에서 한다.

```c#
public int itemCount;
```



```c#
    void OnTriggerEnter(Collider other)
	{
        if(other.name == "Player")
		{
            PlayerBall player = other.GetComponent<PlayerBall>();
            player.itemCount++;
            gameObject.SetActive(false); // 먹었으면 끈다.
		}
	}
```

**SetActivate(bool)**: 오브젝트 활성화 함수



게임 돌리고 아이템 먹으면 itemCount가 2개 된다.

사운드도 넣어보자.



Add Component - Audio Source



**Audio Source**: 사운드 재생 컴포넌트, AudioClip 필요

사운드 없으면 Window - Asset Store 가서 음향 - 효과음



Asset Store - Login - 적절한 음향 검색 구입(0$, 카드 등록 필요)및 다운로드, 

Package Manager에서 우측 하단의 Download 버튼 클릭

다운로드 후 Import하면 된다.



CasualGameSounds 폴더 내의 28번 소리 선택.

Hierarchy에서 Item(아이템)선택한 후, 28번 소리를 Drag & Drop으로 Item의 Inspector에 옮긴다.

볼륨 조절 Volume에서 가능.



Play On Awake - 맨 처음 시작할 때 재생시킨다는 버튼.

해제하자.



ItemCan.cs

```c#
public class ItemCan : MonoBehaviour
{
    public float rotateSpeed;
    AudioSource audio;

    void Awake()
	{
        audio.GetComponent<AudioSource>();
	}

    void Update()
    {
        // transform.Rotate(new Vector3(0, 1, 0));
        transform.Rotate(Vector3.up * rotateSpeed * Time.deltaTime, Space.World);
    }

    void OnTriggerEnter(Collider other)
	{
        if(other.name == "Player")
		{
            PlayerBall player = other.GetComponent<PlayerBall>();
            player.itemCount++;
            audio.Play();
            gameObject.SetActive(false); // 먹었으면 끈다.
		}
	}
}
```

AudioSource audio 등등 변경

이렇게 하면 안 된다.



ItemCan.cs -> PlayerBall로 옮기기

PlayerBall.cs

```c#
public class PlayerBall : MonoBehaviour
{
    public float jumpPower;
    // public float jumpPower = 30;
    public int itemCount;
    bool isJump;
    Rigidbody rigid;
    AudioSource audio;

    void Awake()
	{
        isJump = false;
        rigid = GetComponent<Rigidbody>();
        audio = GetComponent<AudioSource>();
    }

    void Update()
	{
        if(Input.GetButtonDown("Jump") && !isJump)
		{
            isJump = true;
            rigid.AddForce(new Vector3(0, jumpPower, 0), ForceMode.Impulse);
        }
	}

    void FixedUpdate()
    {
        float h = Input.GetAxisRaw("Horizontal");
        float v = Input.GetAxisRaw("Vertical");

        rigid.AddForce(new Vector3(h, 0, v), ForceMode.Impulse);
    }

    void OnCollisionEnter(Collision collision)
	{
        if(collision.gameObject.name == "Floor")
		{
            isJump = false;
		}
	}


    void OnTriggerEnter(Collider other)
    {
        if (other.name == "Item")
        {
            itemCount++;
            audio.Play();
            other.gameObject.SetActive(false); // 먹었으면 끈다.
        }
    }
    
}
```



ItemCan.cs

```c#
public class ItemCan : MonoBehaviour
{
    public float rotateSpeed;

    void Update()
    {
        // transform.Rotate(new Vector3(0, 1, 0));
        transform.Rotate(Vector3.up * rotateSpeed * Time.deltaTime, Space.World);
    }
}
```



아이템 사라지면서 소리내기 완료



아이템 복사 -> 원본은 사라지는데, 아이템은 안사라짐

Tag: 오브젝트를 구분하는 단순한 문자열



아이템 다 선택 - Add Tag

Tags - + 버튼 눌러서 태그 생성

Item



shift 누른 뒤 다중선택해서

Tag 바꿔주면 된다.

Player의 tag는 Player..



이제 이름으로 비교하지 않고, 태그로 비교한다.

```c#
if (other.tag == "Item")
```



Player Ball(Script) - ItemCount 4인 것 확인 완료



Player, Item 상호작용도 배웠다.

카메라 보는 것 불편하죠?



## 4.카메라

카메라가 player 따라가도록 하자.



카메라 위치 조절 후

메인 카메라 전용 Script를 생성한다.



보통 UI, Camera 관련 업데이트는 Update 이후에 발생하는 **LateUpdate**에서 발생한다.



Script에서, Unity의 Hierarchy에서 player를 찾고 싶다면?

```c#
playerTransform = GameObject.FindGameObjectWithTag("Player").transform;
```

오브젝트라서 뒤에 transform 추가



```c#
public class CameraMove : MonoBehaviour
{
    Transform playerTransform;

    void Awake()
    {
        playerTransform = GameObject.FindGameObjectWithTag("Player").transform;
    }

    // UI, Camera 관련 업데이트
    void LateUpdate()
    {
        transform.position = playerTransform.position; // 1인칭
    }
}
```

이렇게 하면 1인칭 시점..



카메라, 공 벡터만큼의 거리가 있으면 좋을 듯.

```c#
public class CameraMove : MonoBehaviour
{
    Transform playerTransform;
    Vector3 Offset;

    void Awake()
    {
        playerTransform = GameObject.FindGameObjectWithTag("Player").transform;
        Offset = transform.position - playerTransform.position;
    }

    // UI, Camera 관련 업데이트
    void LateUpdate()
    {
        // transform.position = playerTransform.position; // 1인칭
        transform.position = playerTransform.position + Offset; // 3인칭
    }
}
```

Offset으로 설정해준다.





## 5.결승점

Hierarchy에서 우클릭 - 3D Object - Cylinder로 결승점 만들자.



색 조절..

투명하게 만든다 - Rendering Mode - Transparent

Emission 체크하고 색 조절하면 빛을 낸다.



충돌하는 애가 아니므로 isTrigger 체크해준다.

Finish라고 태그 달아준다.



이후 PlayerBall.cs쪽으로 간다.

아이템 다 먹고 Finish로 가면 완료

다 못먹은 상태면 처음부터 다시 이런 식으로

-> 아이템 갯수 세줘야 한다.

세는 오브젝트가 있어야겠군.

Hierarchy - 우클릭 - CreateEmpty



형태가 없고 전반적인 로직을 가진 오브젝트를 **매니저**로 지정

보이지 않지만, 게임 구성요소를 관리한다.



매니저 관련한 Script도 만들자.

GameManagerLogic.cs

```c#
public class GameManagerLogic : MonoBehaviour
{
    public int TotalItemCount;
}
```

이것만 들어가면 됨.



이제 PlayerBall.cs쪽에서 TotalItemCount를 찾는다.

Find 계열 함수는 CPU 부하를 초래할 수 있으므로 피하는 것을 권장한다.

PlayerBall.cs 의 원소로 저장해도 된다.

```c#
	public GameManagerLogic manager;

    void OnTriggerEnter(Collider other)
    {
        if (other.tag == "Item")
        {
            itemCount++;
            audio.Play();
            other.gameObject.SetActive(false); // 먹었으면 끈다.
        }
        else if (other.tag == "Item")
        {
            if (itemCount == manager.TotalItemCount)
            {
                // 게임 클리어

            }
            else
			{
                // 재시작
			}
        }
    }
```

이렇게 하고, Hierarchy의 Player의 Manager에

Hierarchy의 GameManager를 Drag&Drop





## 6.장면이동

SceneManager: 장면을 관리하는 기본 클래스

LoadScene(): 주어진 장면을 불러오는 함수



Scene 확인

Project에서 Assets - Scenes



Scene을 불러오려면 꼭 Build Setting에서 추가!

파일 - Build Setting [ctrl + shift + B]

Add Open Scenes 후 체크하기



새로운 신이 필요하다면?

File - Save

File - Save Project로 저장 이후 새로운 것 만들기.

File - New Scene



재시작 - Example1_0

완료(새로운 신) - Example 1_1



```c#
    void OnTriggerEnter(Collider other)
    {
        if (other.tag == "Item")
        {
            itemCount++;
            audio.Play();
            other.gameObject.SetActive(false); // 먹었으면 끈다.
        }
        else if (other.tag == "Item")
        {
            if (itemCount == manager.TotalItemCount)
            {
                // 게임 클리어
                SceneManager.LoadScene("Example1_1");
            }
            else
			{
                // 재시작
                SceneManager.LoadScene("Example1_0");
			}
        }
    }
```



다 하고 나서 Scene Build Setting에 추가

안하면 에러 발생



다먹고 가면.. GameManager의 totalCount가 0이라서 재시작ㅠ

4로 조작하고 목표치로 가면 Clear



스테이지 구현 완료

남은 것 - 지형, 몇 스테이지인지

```c#
public class GameManagerLogic : MonoBehaviour
{
    public int TotalItemCount;
    public int stage;
}
```



```c#
        else if (other.tag == "Finish")
        {
            if (itemCount == manager.TotalItemCount)
            {
                // 게임 클리어
                SceneManager.LoadScene("Example1_" + (manager.stage + 1).ToString());
            }
            else
			{
                // 재시작
                SceneManager.LoadScene("Example1_" + manager.stage);
			}
        }
```





## 7.스테이지

다 스킵하시네.. 순수 노가다



떨어지면 무한으로 떨어진다 ㅠㅠ

떨어지면 재시작해야

Gamemanager - Add Component - Box Collider



낭떠러지

맵 밑에 y축 빼고 사이즈 아주 크게 한다.

isTrigger 체크

여기에 닿게 되면..



GameManagerLogic.cs

```c#
using UnityEngine.SceneManagement;

public class GameManagerLogic : MonoBehaviour
{
    public int TotalItemCount;
    public int stage;

    private void OnTriggerEnter(Collider other)
	{
        if(other.gameObject.tag == "Player")
		{
			SceneManager.LoadScene(0);
		}
	}
}
```

LoadScene의 매개변수는 장면 순서(int)도 가능.

[BuildSettings 오른쪽에 뜨는 번호들]





## 8.UI

Hierarchy - UI - Image

2D 펼쳐두고.. 기본 이미지



텍스트도 추가

Player Item Test, Stage Item Test



GameManagerLogic.cs

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class GameManagerLogic : MonoBehaviour
{
    public int TotalItemCount;
    public int stage;
	public Text stageCountText;
	public Text playerCountText;

	void Awake()
	{
		stageCountText.text = "/ " + TotalItemCount;
	}

	public void GetItem(int count)
	{
		playerCountText.text = count.ToString();
	}

    void OnTriggerEnter(Collider other)
	{
        if(other.gameObject.tag == "Player")
		{
			SceneManager.LoadScene(0);
		}
	}
}
```



PlayerBall.cs

```c#
    void OnTriggerEnter(Collider other)
    {
        if (other.tag == "Item")
        {
            itemCount++;
            audio.Play();
            other.gameObject.SetActive(false); // 먹었으면 끈다.
            manager.GetItem(itemCount);
        }
        else if (other.tag == "Finish")
        {
            if (itemCount == manager.TotalItemCount)
            {
                // 게임 클리어
                SceneManager.LoadScene("Example1_" + (manager.stage + 1).ToString());
            }
            else
			{
                // 재시작
                SceneManager.LoadScene("Example1_" + manager.stage);
			}
        }
    }
```



실행시키려면 Unity의 Hierarchy - GameManager - GameManagerLogic의 

Stage Item Test, Player Item Test 비어있는거에다가

Hierarchy의 Stage Item Test, Player Item Test를 Drag & Drop





### Build

여태껏 만들었던 것 빌드 가능.

파일 - Build Setting [ctrl + shift + B]

Build 버튼 선택

저장할 곳 선택

만들었던 Scene들이 Build된다.



실행 파일이 나온다.



이렇게 첫 번째 예제 끝.

