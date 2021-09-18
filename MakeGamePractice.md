

practice 5

Actor.cs

```c#
public class Actor
{
	public int id;
	public string name;
	public string title;
	public string weapon;
	public float strength;
	public int level;

	public string Talk()
	{
		return "대화를 걸었습니다.";
	}


	public string HasWeapon()
	{
		return weapon;
	}

	public void LevelUp()
	{
		level = level + 1;
	}

}

```



Player.cs

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player : Actor
{
    public string move()
	{
		return "플레이어는 움직입니다.";
	}
}

```



practice5.cs

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class practice5 : MonoBehaviour
{
    int health = 30;

    void Start()
    {
        /*
        Debug.Log("Hello Unity!");


        // 1. 변수
        int level = 5;
        float strength = 15.5f;
        string playerName = "나검사";
        bool isFullLevel = false;

        Debug.Log("용사의 이름은?");
        Debug.Log(playerName);
        Debug.Log("용사의 레벨은?");
        Debug.Log(level);
        Debug.Log("용사의 힘은?");
        Debug.Log(strength);
        Debug.Log("용사는 만렙인가?");
        Debug.Log(isFullLevel);


        // 2. 그룹형 변수
        string[] monsters = { "슬라임", "사막뱀", "악마" };

        Debug.Log("맵에 존재하는 몬스터");
        Debug.Log(monsters[0]);
        Debug.Log(monsters[1]);
        Debug.Log(monsters[2]);

        int[] monsterLevel = new int[3];
        monsterLevel[0] = 1;
        monsterLevel[1] = 6;
        monsterLevel[2] = 20;

        Debug.Log("맵에 존재하는 몬스터 레벨");
        Debug.Log(monsterLevel[0]);
        Debug.Log(monsterLevel[1]);
        Debug.Log(monsterLevel[2]);


        // 리스트
        List<string> items = new List<string>();
        items.Add("생명물약30");
        items.Add("aksk물약30");

        Debug.Log("가지고 있는 아이템");
        Debug.Log(items[0]);
        Debug.Log(items[1]);

        // 지우고 싶은 아이템의 번지수
        items.RemoveAt(0);

        Debug.Log("가지고 있는 아이템");
        Debug.Log(items[0]);
        Debug.Log(items[1]);


        // 3. 연산자
        int exp = 1500;

        exp = 1500 + 320;
        exp = exp - 10;
        level = exp / 300;
        strength = level * 3.1f;

        
        Debug.Log("용사의 총 경험치는?");
        Debug.Log(exp);
        Debug.Log("용사의 레벨은?");
        Debug.Log(level);
        Debug.Log("용사의 힘은?");
        Debug.Log(strength);

        int nextExp = 300 - (exp % 300);
        //Debug.Log("다음 레벨까지 남은 경험치는?");
        Debug.Log(nextExp);

        string title = "전설의";
        //Debug.Log("용사의 이름은?");
        Debug.Log(title + " " + playerName);

        int fullLevel = 99;
        isFullLevel = level == fullLevel;
        //Debug.Log("용사는 만렙입니까?" + isFullLevel);

        bool isEndTutorial = level > 10;
        //Debug.Log("튜토리얼이 끝난 용사입니까? " + isEndTutorial);

        int health = 30;
        int mana = 25;
        // bool isBadCondition = health <= 50 && mana <= 20;
        bool isBadCondition = health <= 50 || mana <= 20;
        //Debug.Log("용사의 상태가 나쁩니까?" + isBadCondition);

        string condition = isBadCondition ? "나쁨" : "좋음";


        // 4. 키워드
        // int, float, string, bool, new, List

        // 5. 조건문
        
        if (condition == "나쁨")
        {
            Debug.Log("플레이어 상테가 나쁘니 아이템을 사용하세요.");
        }
        else
        {
            Debug.Log("플레이어 상태가 좋습니다.");
        }

        if (isBadCondition && items[0] == "생명물약30")
        {
            items.RemoveAt(0);
            health += 30;
            Debug.Log("생명포션30을 사용하였습니다.");
        }
        else if (isBadCondition && items[0] == "마나물약30")
        {
            items.RemoveAt(0);
            mana += 30;
            Debug.Log("마나포션30을 사용하였습니다.");
        }

        switch (monsters[1])
        {
            case "슬라임":
            case "사막뱀":
                Debug.Log("소형 몬스터가 출현!");
                break;
            case "악마":
                Debug.Log("중형 몬스터가 출현!");
                break;
            case "골렘":
                Debug.Log("대형 몬스터가 출현!");
                break;
            default:
                Debug.Log("??? 몬스터가 출현!");
                break;
        }
        

        // 6. 반복문
        
        while (health > 0) {
            health--;
            if (health > 0)
            {
                Debug.Log("독 데미지를 입었습니다." + health);
            }
            else
            {
                Debug.Log("사망하였습니다.");
            }

            if (health == 10)
            {
                Debug.Log("해독제를 사용합니다.");
                break;
            }
        }
        

        
        for (int count = 0; count < 10; count++) 
        {
            health++;
            Debug.Log("붕대로 치료 중..." + health);
        }

        for (int index=0; index < monsters.Length; index++)
		{
            Debug.Log("이 지역에 있는 몬스터: " + monsters[index]);
		}

        foreach( string monster in monsters)
		{
            Debug.Log("이 지역에 있는 몬스터 :" + monster);
		}
        


        // 7. 함수 (메소드)
        health = Heal(health);

        for (int index = 0; index < monsters.Length; index++) 
        {
            Debug.Log("용사는 " + monsters[index] + "에게" + Battle(monsterLevel[index]));
        }


        // 8. 클래스
        Player player = new Player();
        player.id = 0;
        player.name = "나법사";
        player.title = "현명한";
        player.strength = 2.4f;
        player.weapon = "나무 지팡이";
        Debug.Log(player.Talk());
        Debug.Log(player.HasWeapon());

        player.LevelUp();
        Debug.Log(player.name + "의 레벨은 " + player.level + " 입니다.");
        Debug.Log(player.move());
    }

    // 7. 함수 (메소드)
    int Heal(int currentHealth) 
    {
        currentHealth += 10;
        Debug.Log("힐을 받았습니다. " + currentHealth);
        return currentHealth;
    }

    void Heal2()
	{
        health += 10;
        Debug.Log("힐을 받았습니다. " + health);
	}

    string Battle(int monsterLevel)
	{
        string result;

        if (monsterLevel >= monsterLevel)
            result = "이겼습니다.";
        else
            result = "졌습니다.";

        return result;*/
	}
}

```





practice 6

LifeCycle.cs

```c#
using UnityEngine;

public class LifeCycle : MonoBehaviour
{
	// 게임 오브젝트 생성할 때, 최초 실행
	void Awake() 
	{
		Debug.Log("플레이어 데이터가 준비되었습니다.");
	}

	// 게임 오브젝트가 활성화 되었을 때
	void OnEnable()
	{
		Debug.Log("플레이어가 로그인했습니다.");
	}

	// 업데이터 시작 직전, 최초 실행
	void Start()
	{
		Debug.Log("사냥 장비를 챙겼습니다.");
	}

	// 물리 연산 업데이트
	void FixedUpdate()
	{
		Debug.Log("이동~");
	}

	// 물리 연산에 관련된 연산을 제외한, 나머지, 주기적으로 변하는 로직을 넣을 때 사용하는 함수.
	void Update()
	{
		Debug.Log("몬스터 사냥!");
	}

	// 모든 업데이트 끝난 후
	void LateUpdate()
	{
		Debug.Log("경험치 획득.");
	}

	// 게임 오브젝트가 비활성화 되었을 때
	void OnDisable()
	{
		Debug.Log("플레이어가 로그아웃했습니다.");
	}

	// 게임 오브젝트가 삭제될 때
	void OnDestroy()
	{
		Debug.Log("플레이어 데이터를 해제하였습니다.");
	}
}
```





practice7

practice7.cs

```c#
using UnityEngine;

public class practice7 : MonoBehaviour
{

    //void Update()
    //{
        // 1. 키보드, 마우스 입력

        // if (Input.anyKeyDown)
        //     Debug.Log("플레이어가 아무 키를 눌렀습니다.");

        // KeypadEnter와 Return 같은 말
        /*
        if (Input.GetKeyDown(KeyCode.Return))
            Debug.Log("아이템을 구입하였습니다.");

        if (Input.GetKey(KeyCode.LeftArrow))
            Debug.Log("왼쪽으로 이동 중");

        if (Input.GetKeyUp(KeyCode.RightArrow))
            Debug.Log("오른쪽 이동을 멈추었습니다.");*/

        /* if (Input.GetMouseButtonDown(0))
            Debug.Log("미사일 발사!");

        if (Input.GetMouseButton(0))
            Debug.Log("미사일 모으는 중...");

        if (Input.GetMouseButtonUp(0))
            Debug.Log("슈퍼 미사일 발사!!"); */

        /* if (Input.GetButtonDown("Jump"))
            Debug.Log("점프!");

        if (Input.GetButton("Jump"))
            Debug.Log("점프 모으는 중...");

        if (Input.GetButtonUp("Jump"))
            Debug.Log("슈퍼 점프!!"); */

        /*
        if (Input.GetButton("Horizontal"))
		{
            Debug.Log("횡 이동 중..." + Input.GetAxis("Horizontal"));
		}


        if (Input.GetButton("Vertical"))
        {
            Debug.Log("종 이동 중..." + Input.GetAxis("Vertical"));
        }*/
   // }

    void Start()
    {
        // 2. 오브젝트 이동
        // Vector3 vec = new Vector3(5, 0, 0);
        // transform.Translate(vec);
    }

    void Update()
	{
        // 2. 오브젝트 이동
        // Vector3 vec = new Vector3(0, 0.1f, 0);
        // transform.Translate(vec);

        /*Vector3 vec = new Vector3(
            Input.GetAxis("Horizontal"),
            Input.GetAxis("Vertical"),
            0);*/
        /* Vector3 vec = new Vector3(
            Input.GetAxisRaw("Horizontal"),
            Input.GetAxisRaw("Vertical"),
            0);*/
        //transform.Translate(vec);
    }
}

```





practice8

practice8.cs

```c#
using UnityEngine;

public class Move : MonoBehaviour
{
    Vector3 target = new Vector3(8, 1.5f, 0);

    void Update()
    {
        // 1. MoveTowards
        // transform.position = Vector3.MoveTowards(transform.position, target, 2f);

        // 2. SmoothDamp
        // Vector3 velo = Vector3.zero;
        // Vector3 velo = Vector3.up * 50; 이거 쓰면 목표지점 의미가 없어져서 잘 안 쓴다.

        //transform.position = Vector3.SmoothDamp(transform.position, target, ref velo, 0.1f);

        // 3. Lerp (선형 보간)
        // transform.position = Vector3.Lerp(transform.position, target, 0.05f);

        // 4. SLerp (구면 선형 보간)
        transform.position = Vector3.Slerp(transform.position, target, 0.05f);
    }
}
```





practice9

DeltaTime.cs

```c#
using UnityEngine;

public class DeltaTime : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        Vector3 vec = new Vector3(
            Input.GetAxisRaw("Horizontal") * Time.deltaTime,
            Input.GetAxisRaw("Vertical") * Time.deltaTime);
            transform.Translate(vec);
      }
}
```





practice10

MyBall.cs

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MyBall : MonoBehaviour
{
    Rigidbody rigid; // 선언

    // Start is called before the first frame update
    void Start()
    {
        rigid = GetComponent<Rigidbody>();
        // rigid.AddForce(Vector3.up * 50, ForceMode.Impulse);
    }

    // Update is called once per frame
    void FixedUpdate()
    {
        // #1. 속력 바꾸기
        // rigid.velocity = Vector3.right;
        // rigid.velocity = Vector3.left;
        // rigid.velocity = new Vector3(2, 4, 3); 

        // #2. 힘을 가하기
        
        if (Input.GetButtonDown("Jump"))
		{
            rigid.AddForce(Vector3.up * 5, ForceMode.Impulse);
            // Debug.Log(rigid.velocity);
        }

        Vector3 vec = new Vector3(
            Input.GetAxisRaw("Horizontal")
            , 0, Input.GetAxisRaw("Vertical")
            );
        rigid.AddForce(vec, ForceMode.Impulse);

        // #3. 회전력
        // rigid.AddTorque(Vector3.back);
        // rigid.AddTorque(Vector3.up);
    }

    private void OnTriggerStay(Collider other)
    {
        if (other.name == "Cube")
            rigid.AddForce(Vector3.up * 2, ForceMode.Impulse);
    }

    public void Jump()
	{
        rigid.AddForce(Vector3.up * 20, ForceMode.Impulse);
    }
}

```





practice11

OtherBall.cs

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class OtherBall : MonoBehaviour
{
    MeshRenderer mesh;
    Material mat;

    // Start is called before the first frame update
    void Start()
    {
        mesh = GetComponent<MeshRenderer>();
        mat = mesh.material;
        // mat.color = new Color(0, 0, 0);
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    private void OnCollisionEnter(Collision collision)
	{
        if(collision.gameObject.name == "MyBall")
            mat.color = new Color(0, 0, 0);
	}

    private void OnCollisionExit(Collision collision)
    {
        if (collision.gameObject.name == "MyBall")
            mat.color = new Color(1, 1, 1);
    }

    /*
    private void OnCollisionStay(Collision collision)
    {
        
    }

    private void OnCollisionExit(Collision collision)
    {
        
    }
    */
}

```

