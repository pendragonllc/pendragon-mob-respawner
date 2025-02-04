using System.Collections.Generic;
using UnityEngine;

public class MobsRespawn : MonoBehaviour
{

    [SerializeField] private List<GameObject> _mobs;
    private readonly float _respawnCooldown = 120.0f; // Encourage player to find other spawns, explore the map, and prevent player from getting mobbed.
    private float _lastRespawnTime = -10.0f;

    [SerializeField] private float respawnDelay = 3f; // Delay before respawning once MC triggers the collider.
    private bool isRespawning = false;
    private bool IsRespawnCooldownFinished => Time.time > _lastRespawnTime + _respawnCooldown;

    private void Update()
    {
        bool allMobsDead = true;
        foreach (GameObject mob in _mobs)
        {
            if (mob.activeSelf)
            {
                allMobsDead = false;
            }
        }

        if (allMobsDead && !isRespawning)
        {
            isRespawning = true;
            _lastRespawnTime = Time.time; // NOW that the mobs are dead, we can start the respawn countdown.
        }

        if (isRespawning)
        {
            // Help testing if cooldown works.
            Debug.Log($"Respawning time left: {Mathf.Max(0, (_lastRespawnTime + _respawnCooldown) - Time.time)}");
        }

        if (isRespawning && IsRespawnCooldownFinished)
        {
            foreach (GameObject mob in _mobs)
            {
                mob.SetActive(true); // Reactivate all mobs
                // Optional: set the position to be near the MobRespawn object instead of where the mobs last died? :P
            }
            isRespawning = false;
        }
    }
}


