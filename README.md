# ppal-infra

Local development orchestration for the PantryPal MVP. This repository runs one PostgreSQL database and one REST API; it does not create a microservice per repository.

## Workspace layout

Clone the required repositories as sibling directories:

```
pantrypal-workspace/
├── ppal-api/
├── ppal-db/
└── ppal-infra/
```

Run Docker Compose from the ppal-infra directory. The compose file builds the API from ppal-api and mounts the initial database migration and demo seed from ppal-db.

## Start the local stack

1. Install Docker Desktop and start its engine.
2. Copy .env.example to .env if custom local values are needed.
3. Ensure ppal-api and ppal-db have the relevant reviewed feature branches checked out.
4. Run docker compose up --build -d, docker compose ps, and docker compose logs -f api.

The API is available on the host at http://localhost:3000. Verify liveness with http://localhost:3000/health and list products at http://localhost:3000/api/products.

The API port is bound to 127.0.0.1 to avoid exposing the unauthenticated demonstration service on all host interfaces. PostgreSQL is not published to the host network.

## Android emulator

The Android emulator reaches the host machine through 10.0.2.2. For a local native debug build, configure the mobile client's development API URL accordingly and allow cleartext traffic only in the debug environment. Do not reuse HTTP or permissive cleartext settings for a production build.

## Database lifecycle

PostgreSQL initialization scripts run only when the data directory is empty. To reset a disposable demo database, run docker compose down -v and then docker compose up --build -d from this directory. The volume deletion command permanently deletes the local database data.

## Security boundary

The MVP has no authentication and is only intended for local development or a controlled demonstration. Do not expose this stack to the public internet or use sensitive personal data. Before any public deployment, add an explicit security review, authentication/authorization decision, TLS, secret management, backups, and environment-specific operational controls.
