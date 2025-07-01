
## 📘 **Redis: Concepts, Best Practices & Commands**

---

### ✅ **What is Redis?**

* **Redis** = *Remote Dictionary Server*
* It’s an **in-memory key-value data store**
* Super fast for **caching**, **sessions**, **pub/sub**, **queues**, and **ephemeral data**.
* Supports multiple data types: `string`, `hash`, `list`, `set`, `sorted set`, `streams`, `pub/sub channels`.

---

### ⚡ **Why use Redis?**

| Use Case                    | Why Redis?                                              |
| --------------------------- | ------------------------------------------------------- |
| **Session storage**         | Very fast, low-latency, stores session tokens / cookies |
| **Cache**                   | Stores frequently accessed data, reduces DB load        |
| **Rate limiting**           | Track counts with expiry (IP hits, login attempts)      |
| **Pub/Sub**                 | Real-time message broadcasting                          |
| **Queues / Jobs**           | Lightweight job/task queues                             |
| **Leaderboards / counters** | Sorted sets with scores                                 |

---

## 🗂️ **How LoopBack uses Redis (your context)**

* Used **as a KV store** with `kv-redis` connector.
* Stores **JWT refresh tokens** with short TTL.
* Does *not* persist to your main SQL DB — ephemeral data only.
* `SET` writes token.
* `GET` reads token to verify / renew session.
* `SCAN` / `KEYS` for inspecting keys.
* Prefix `_kvModel:` is common when using `kv-redis` connector.

---

## 🔑 **Key concepts**

* **Ephemeral store:** Data **lives in RAM** → if Redis restarts, all keys may be lost (unless persistence is enabled with `AOF` or `RDB` dumps).
* **TTL:** Keys can **expire automatically** (`EXPIRE`).
* **Single-threaded but fast:** Uses async IO — best for small ops.
* **Atomic operations:** Increments, check-and-set, etc.

---

## 🧩 **Basic Redis CLI commands**

| Command              | What it does                                       |
| -------------------- | -------------------------------------------------- |
| `PING`               | Check connection: returns `PONG`                   |
| `SET key value`      | Store value                                        |
| `GET key`            | Get value                                          |
| `DEL key`            | Delete key                                         |
| `EXPIRE key seconds` | Set TTL                                            |
| `TTL key`            | Get TTL                                            |
| `KEYS *`             | Get all keys (not recommended in production)       |
| `SCAN 0`             | Cursor-based key iteration (use instead of `KEYS`) |
| `FLUSHALL`           | Deletes **all keys** (careful!)                    |
| `INFO`               | Show Redis stats                                   |
| `AUTH password`      | Authenticate                                       |
| `SELECT 0`           | Switch database index                              |

---

## ⚙️ **Common Redis patterns**

### ✅ **Storing a refresh token**

```ts
await redis.set(tokenId, JSON.stringify(tokenData));
```

### ✅ **Fetching & verifying**

```ts
const raw = await redis.get(tokenId);
if (!raw) throw new Error('Not found');
const data = JSON.parse(raw);
```

### ✅ **Set with TTL**

```bash
SET key value EX 3600  # Expires in 1 hour
```

Or:

```ts
await redis.setex('session:user:123', 3600, JSON.stringify(data));
```

---

## ⚡ **Production best practices**

✅ **Use TTLs**

* Always expire session keys.
* Prevent stale data filling up RAM.

✅ **Use SCAN instead of KEYS**

* `KEYS *` is blocking and slow for many keys.
* `SCAN` uses a cursor to iterate safely.

✅ **Secure your instance**

* Use `AUTH` with strong password.
* Bind to private network only.
* Enable TLS if used over public internet.

✅ **Monitor memory**

* Redis is RAM-only → OOM kills performance.
* Use `maxmemory` and eviction policies.

✅ **Backups**

* Use `RDB` or `AOF` if data needs persistence.
* For sessions/tokens, you usually don’t persist.

✅ **Cluster when needed**

* For scale: Redis Sentinel for HA.
* Redis Cluster for partitioning data.

---

## 📌 **Common gotchas**

❌ Don’t store huge blobs (like images/files).

❌ Don’t forget TTL — or RAM will fill up.

❌ Don’t `KEYS *` in prod scripts.

❌ Remember: **KV store ≠ SQL** — no querying by value.

---

## 🧑‍💻 **Example Redis flow (Your LoopBack)**

1. **User logs in**

   * `SET refreshTokenId` → stores `{accessToken, refreshToken, userId}`

2. **Client calls `/refresh-token`**

   * `GET refreshTokenId` → verify → issue new `accessToken`

3. **Optional: Revoke**

   * `DEL refreshTokenId`

---

## 🗝️ **How to connect**

```bash
redis-cli -h <host> -p <port> -a <password>
```

---

## ✅ **Checklist**

* [ ] Use `.env` for Redis config
* [ ] Secure with TLS if needed
* [ ] Add `console.log` when debugging `.set` / `.get`
* [ ] Use `SCAN` to inspect keys
* [ ] Use `EXPIRE` to prevent stale data

---

## 📚 **Useful commands recap**

```bash
# Connect
redis-cli -h your-redis-host -p your-port -a your-password

# Ping
PING

# Set a key
SET mykey "hello world"

# Get it back
GET mykey

# Set with expiry
SETEX mykey 60 "temporary"

# Check TTL
TTL mykey

# List keys safely
SCAN 0

# Delete key
DEL mykey

# Flush all keys (CAREFUL!)
FLUSHALL
```

---

## ✅ **Summary**

Redis is **fast**, **simple**, and perfect for:

* **Sessions**
* **Tokens**
* **Rate limiting**
* **Lightweight caching**

Your `refreshTokenRepository` just wraps this with a LoopBack KV connector.
Always remember: *RAM is precious, TTLs are your friend!*

---

## 📎 **Keep learning**

* Official docs: [redis.io](https://redis.io)
* LoopBack KV docs: [LoopBack KeyValue](https://loopback.io/doc/en/lb4/Key-value-connector.html)
* Sentinel & Clustering for HA

---

### 📌 **Your core takeaway**

> ✅ *Use Redis for fast, ephemeral, key-value storage. Use SQL for long-term records.*
> ✅ *Secure, monitor, and expire keys responsibly.*
