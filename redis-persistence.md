Yes, you **already have Redis data persistence enabled** in your Docker Compose file, thanks to this line:

```yaml
volumes:
  - redis-data:/data
```

This mounts a Docker-managed volume (`redis-data`) to the `/data` directory inside the Redis container. Redis stores its persistent data (RDB snapshots and/or AOF logs) in `/data`. As a result, **your data will survive container restarts, reboots, and even recreating the container**—as long as you don't delete the volume itself [[1]](https://sliplane.io/blog/setting-up-redis-with-docker-compose).

### How Redis Persistence Works

Redis supports two main persistence mechanisms:
- **RDB snapshots** (default): Periodically saves the dataset to disk.
- **AOF (Append Only File)**: Logs every write operation received by the server.

By default, the official Redis image uses RDB snapshots, which are stored in `/data/dump.rdb`.

### Improving Persistence (Optional)

If you want to **customize how often Redis saves data**, you can modify the `command` in your Compose file. For example:

```yaml
command: ["redis-server", "--requirepass", "yourpassword", "--save", "60", "1"]
```
This means: save a snapshot every 60 seconds if at least 1 key changed.

If you want **even more durability**, you can enable AOF:

```yaml
command: ["redis-server", "--requirepass", "yourpassword", "--appendonly", "yes"]
```

### No Changes Needed in Your Python Code

Your Python class (`SessionRedisChatHistory`) does **not** need any changes for persistence. Persistence is handled entirely by Redis and Docker. Your class just interacts with Redis as usual.

---

**Summary:**  
- Your current Docker Compose setup already persists Redis data across reboots.
- For even more robust persistence, consider enabling AOF in the Redis config.
- No changes are needed in your Python code for persistence [[1]](https://sliplane.io/blog/setting-up-redis-with-docker-compose).

