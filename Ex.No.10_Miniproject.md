# Ex.No: 10  Implementation of 2D game of Catch the Falling Objects
### DATE: 17/11/2025                                                                         
### REGISTER NUMBER : 212222110018

### AIM: 
To develop a Catch the Falling Objects 2D game in Unity, where the player controls a basket to catch good objects while avoiding harmful ones.

### Algorithm:
```
1. Start a new 2D project in Unity.
2. Create sprites for basket, stars (good objects), and bombs (bad objects).
3. Add a BasketController script to move the basket horizontally using keyboard or mouse input.
4. Create falling object prefabs and assign Rigidbody2D + collider.
5. Create a Spawner script to randomly spawn falling objects at the top of the screen.
6. Add a GameManager script to manage score, lives, audio, and Game Over logic.
7. Detect collisions between the basket and falling objects using triggers.
8. Increase score for collecting stars and reduce lives when catching bombs or missing good objects.
9. Create UI elements to display score, lives, and Game Over screen.
10. Test the game and adjust speed, difficulty, and spawn rate.

```  
### Program:
## BasketController.cs
```
using UnityEngine;

public class BasketController : MonoBehaviour
{
    public float speed = 10f;
    private Camera cam;

    void Start()
    {
        cam = Camera.main;
    }

    void Update()
    {
        float move = Input.GetAxis("Horizontal") * speed * Time.deltaTime;
        Vector3 pos = transform.position + new Vector3(move, 0, 0);

        float halfWidth = cam.orthographicSize * cam.aspect;
        pos.x = Mathf.Clamp(pos.x, -halfWidth + 0.5f, halfWidth - 0.5f);

        transform.position = pos;
    }
}

```

## FallingObject.cs
```
using UnityEngine;

public class FallingObject : MonoBehaviour
{
    public bool isBad = false;
    public int scoreValue = 10;

    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Basket"))
        {
            if (!isBad)
                GameManager.Instance.AddScore(scoreValue);
            else
                GameManager.Instance.LoseLife();

            Destroy(gameObject);
        }
        else if (other.CompareTag("Ground"))
        {
            if (!isBad)
                GameManager.Instance.LoseLife();

            Destroy(gameObject);
        }
    }
}

```
## Spawner.cs
```
using UnityEngine;
using System.Collections;

public class Spawner : MonoBehaviour
{
    public GameObject[] objects;
    public float interval = 1f;

    void Start()
    {
        StartCoroutine(SpawnRoutine());
    }

    IEnumerator SpawnRoutine()
    {
        while (true)
        {
            SpawnOne();
            yield return new WaitForSeconds(interval);
        }
    }

    void SpawnOne()
    {
        int index = Random.Range(0, objects.Length);
        Instantiate(objects[index], 
            transform.position + new Vector3(Random.Range(-8f, 8f), 0, 0), 
            Quaternion.identity);
    }
}

```
## GameManager.cs
```
using UnityEngine;
using UnityEngine.UI;

public class GameManager : MonoBehaviour
{
    public static GameManager Instance;
    public Text scoreText;
    public Text livesText;
    public GameObject gameOverPanel;

    private int score = 0;
    private int lives = 3;

    void Awake()
    {
        Instance = this;
        gameOverPanel.SetActive(false);
        UpdateUI();
    }

    public void AddScore(int val)
    {
        score += val;
        UpdateUI();
    }

    public void LoseLife()
    {
        lives--;
        UpdateUI();
        if (lives <= 0)
            GameOver();
    }

    void UpdateUI()
    {
        scoreText.text = "Score: " + score;
        livesText.text = "Lives: " + lives;
    }

    void GameOver()
    {
        gameOverPanel.SetActive(true);
        Time.timeScale = 0f;
    }
}

```
### Output:
![WhatsApp Image 2025-11-17 at 09 02 53_3a65bf60](https://github.com/user-attachments/assets/6d74fd00-03ec-4ffe-b92c-0becc43f88c7)

### Result:
Thus the Catch the Falling Objects game was successfully developed using Unity and adopted basic game physics and object-trigger interaction AI logic.
