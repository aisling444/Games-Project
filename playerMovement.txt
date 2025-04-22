using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 5f;
    private Vector2 moveDirection;

    // Footstep sound-related variables
    public AudioClip footstepSound;  // Footstep sound clip
    private AudioSource audioSource; // AudioSource for playing the sound
    private float stepTimer = 0.0f; // Timer to control the step interval
    public float stepInterval = 0.5f; // Interval between footsteps

    void Start()
    {
        // Get the AudioSource component
        audioSource = GetComponent<AudioSource>();
    }

    void Update()
    {
        // Get input from the player (WASD or arrow keys)
        moveDirection.x = Input.GetAxisRaw("Horizontal");
        moveDirection.y = Input.GetAxisRaw("Vertical");
        
           

        


        // Move the player
        transform.Translate(moveDirection * moveSpeed * Time.deltaTime);

        // Handle footstep sound logic
        HandleFootsteps();
    }

    void HandleFootsteps()
    {
        if (moveDirection.magnitude > 0)  // Check if player is moving
        {
            stepTimer += Time.deltaTime;

            if (stepTimer >= stepInterval)  // If enough time has passed, play a footstep sound
            {
                audioSource.PlayOneShot(footstepSound);
                stepTimer = 0.0f;  // Reset the timer
            }
        }
    }

    void OnTriggerEnter2D(Collider2D other)
    {
        Debug.Log("Collision detected with: " + other.name);
        if (other.CompareTag("keyPiece"))  // Assuming you tag key pieces as "KeyPiece"
        {
            Debug.Log("KeyPiece Collected!");
            // Destroy the key piece when collected
            Destroy(other.gameObject);
            KeyPiece.keyPiecesCollected++;  // Increment the key pieces collected (replace with your logic)
        }
    }
}






