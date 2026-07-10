cat > ~/kb/templates/note-template.md <<'EOF'
# Title
Docker Build Tutorial

## Purpose
Build and Run a basic docker nginx webapp

## Prereqs
Accounts, IPs, tools, access, assumptions.

## Steps
1. Create project layout
2. Code DockerFile
3. Run build and run commands
4. Use follow up commands
5. Switch to bind mount for faster learning

## Commands
```bash
# DockerFile Code:
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
COPY style.css /usr/share/nginx/html/style.css

EXPOSE 80

# Build and run commands:
docker build -t ben-nginx-site . #-t gives name
docker run -d --name ben-site -p 8080:80 ben-nginx-site #-d runs detached mode

# Follow up Commands:
docker ps
docker logs ben-site
docker stop ben-site
docker rm ben-site
docker images

#Create a bind mount:
docker run -d \
  --name ben-site-live \
  -p 8080:80 \
  -v "$(pwd)":/usr/share/nginx/html:ro \ #:ro does read only
  nginx:alpine

```

## Verification
```bash
# checks / show commands / curl tests / ping tests
```

## Rollback
```bash
# undo or recovery commands
```

## Notes
Extra context, gotchas, links, ticket refs.
