# n8n docker container for developers

Features:
- Autoreload Nodes descriptions after Node rebuild
- mitmproxy

## Setup

1. Put below changes to your Node repo for autoreload feature

./scripts/package-reload.sh:
```bash
#!/usr/bin/env bash

curl --request POST \
  --url http://localhost:5678/rest/dev/install \
  --header 'accept: application/json' \
  --header 'content-type: application/json' \
  --data "{\"name\": \"$1\"}"
```

Package.json:
```json
  "scripts": {
    "dev": "tsc-watch --onSuccess './scripts/package-reload.sh @digital-boss/n8n-nodes-outputs-test'",
  },
  "devDependencies": {
    "tsc-watch": "^5.0.3",
  },
```

2. See REPOS_DIR in docker-compose.yaml, change that line if your node repos is outside home folder. If they are inside, then move to next steps/
3. `REPOS_DIR=path/to/repos make up`
4. http://localhost:5678 - n8n workspace
5. http://localhost:8081 - mitmproxy
6. `make stop`, `make start`, `make down`. Se other make commands in Makefile
