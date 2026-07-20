cat > ~/kb/templates/note-template.md <<'EOF'
# Title
Docker Compose

## Purpose
Commands for using docker compose

## Prereqs
Accounts, IPs, tools, access, assumptions.

## Commands
```bash
#Build reliably
docker compose up --build -d

#Take down 
docker copose down --remove-orphans

docker compose ps

docker compose logs nginx

docker compose exec <containername> /bin/sh

#docker network commands

docker network ls

docker network inspect <project>_default

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
