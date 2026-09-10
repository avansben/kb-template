# Docker Resource Cleanup

## Purpose
Removing unused containers, images, and volumes to reclaim disk space.

## Prereqs
Docker installed and running.

## Steps
1. Identify unused resources.
2. Prune unused data.
3. Remove specific large images.

## Commands
```bash
# List all containers (including stopped)
docker ps -a

# Remove all stopped containers
docker container prune

# Remove all unused images (dangling)
docker image prune

# Remove all unused volumes
docker volume prune

# The "Nuclear" option: remove all unused containers, networks, and images
docker system prune -a --volumes
```

## Verification
```bash
# Check disk usage of docker
docker system df
```

## Rollback
N/A - Pruned data is permanently deleted.

## Notes
`docker system prune -a` will remove all images that are not associated with at least one container.
