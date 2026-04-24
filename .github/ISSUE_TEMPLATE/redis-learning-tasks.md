
🚀 AZ-204 Redis Project Checklist


🏗️ Infrastructure & Configuration
Provision Azure Cache for Redis: Create a Basic or Standard tier instance via the Azure Portal or CLI.
Configure Eviction Policies: Document the difference between allkeys-lru and volatile-lru in your settings.
Set Up Persistence: (Optional/Premium) Enable RDB or AOF to understand data durability.
Performance Tuning: Configure maxmemory-reserved and maxfragmentationmemory-reserved settings.


💻 Development (StackExchange.Redis)
Connect via ConnectionString: Securely store and retrieve the primary connection string (use Key Vault for bonus points).
Implement Cache-Aside Pattern:
Create logic to check cache first (StringGet).
Implement database fallback for cache misses.
Write back to cache (StringSet).
Manage Data Expiration:
Set absolute expiration (TTL) on product items.
Implement sliding expiration for user sessions.
Handle Complex Types: Practice serializing/deserializing JSON objects into Redis strings.


🔐 Security & Identity
Disable Non-TLS Port: Ensure only port 6380 (SSL) is enabled.
Entra ID Authentication: Configure Redis to use Token-based authentication (RBAC) instead of static Access Keys.
Access Policy: Create a custom Redis user with restricted permissions (e.g., read-only).


⚡ Integration & Monitoring
Redis Triggers: Connect an Azure Function to your Redis instance using a Pub/Sub or Keyspace notification.
Monitor Performance: Check the "Cache Hits vs. Misses" and "CPU usage" in Azure Monitor/Insights.
Local Testing: Use a Docker container running Redis locally for your development environment before pushing to Azure.

⚡ Event-Driven Logic (Redis Triggers)
To ensure the catalog remains synchronized and reactive, we use Azure Functions with Redis Keyspace Notifications.

#### Redis Trigger Scenarios


| Feature | Redis Channel to Trigger On | Project Purpose |
| :--- | :--- | :--- |
| **Proactive Refresh** | `__keyevent@0__:expired` | Automatically refill cache from the DB when TTL expires so the next user hits a "warm" cache. |
| **Inventory Alert** | `__keyevent@0__:set` | Monitor stock levels; trigger a "Low Stock" email or notification if the new value falls below 5. |
| **Data Clean Up** | `__keyevent@0__:del` | Ensure no "ghost" data remains; remove related metadata or promotion flags when a product is deleted. |


