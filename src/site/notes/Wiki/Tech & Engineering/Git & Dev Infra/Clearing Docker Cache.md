---
{"dg-publish":true,"permalink":"/wiki/tech-and-engineering/git-and-dev-infra/clearing-docker-cache/","title":"Clearing Docker Cache","tags":["Guide","Linux","Systeme"],"dg-note-properties":{"type":"Page","collections":"Knowledge & Guides","title":"Clearing Docker Cache","aliases":null,"description":null,"icon":null,"createdAt":"2025-11-29T08:55:39.841Z","lastUpdated":"2026-01-09T06:22:08.936Z","tags":["Guide","Linux","Systeme"],"coverImage":null}}
---

# Clearing Docker Cache
Docker cache can accumulate over time, taking up significant disk space. Here are the most effective commands to clean up Docker cache and free up storage.

## Quick Cleanup Commands

### Complete System Cleanup
```bash
docker system prune -a
```

**What it does:** Removes all unused containers, networks, images (both dangling and unreferenced), and build cache. The `-a` flag ensures that all unused images are removed, not just dangling ones.

> [!Warning] ⚠️ **Warning**
> 
> This is aggressive and will remove all unused Docker resources. Make sure you don't need any stopped containers or unused images.

### Image Cleanup Only
```bash
docker image prune
```

**What it does:** Removes only dangling images (intermediate layers that are no longer referenced by any tagged image).

## More Targeted Cleanup Options

### Remove Unused Containers
```bash
docker container prune
```

### Remove Unused Networks
```bash
docker network prune
```

### Remove Unused Volumes
```bash
docker volume prune
```

### Remove Build Cache
```bash
docker builder prune
```

## Check Disk Usage
Before cleaning up, you can check how much space Docker is using:

```bash
docker system df
```

This shows:
- Images space usage
- Containers space usage
- Local volumes space usage
- Build cache usage

## Best Practices
- **Regular cleanup:** Run `docker system prune` regularly to prevent cache buildup
- **Check first:** Use `docker system df` to see what's taking up space
- **Be selective:** Use specific prune commands if you want to keep certain resources
- **Automation:** Consider adding cleanup commands to your development workflow

## When to Use Each Command
- Use `docker system prune -a` for comprehensive cleanup when disk space is low
- Use `docker image prune` for lighter cleanup that only removes dangling images
- Use specific prune commands when you want to clean only certain resource types


