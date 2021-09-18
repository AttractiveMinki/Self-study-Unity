CameraMove.cs

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

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



GameManagerLogic.cs

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class GameManagerLogic : MonoBehaviour
{
    public int totalItemCount;
    public int stage;
	public Text stageCountText;
	public Text playerCountText;

	void Awake()
	{
		print(stageCountText);
		stageCountText.text = "/ " + totalItemCount;
		print(stageCountText);
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



ItemCan.cs

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

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



PlayerBall.cs

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class PlayerBall : MonoBehaviour
{
    public float jumpPower;
    // public float jumpPower = 30;
    public int itemCount;
    public GameManagerLogic manager;

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
        if(collision.gameObject.tag == "Floor")
		{
            isJump = false;
		}
	}


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
            if (itemCount == manager.totalItemCount)
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
}
```

