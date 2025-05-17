
```yaml
version: '3.8'

  

services:

mongo-auth:

image: mongo:6

container_name: mongo-auth

restart: always

ports:

- "27017:27017"

environment:

MONGO_INITDB_ROOT_USERNAME: ${MONGO_INITDB_ROOT_USERNAME}

MONGO_INITDB_ROOT_PASSWORD: ${MONGO_INITDB_ROOT_PASSWORD}

volumes:

- ./auth-server/init-auth.js:/docker-entrypoint-initdb.d/init-auth.js:ro

- mongo-auth-data:/data/db

  

mongo-event:

image: mongo:6

container_name: mongo-event

restart: always

ports:

- "27018:27017"

environment:

MONGO_INITDB_ROOT_USERNAME: ${MONGO_INITDB_ROOT_USERNAME}

MONGO_INITDB_ROOT_PASSWORD: ${MONGO_INITDB_ROOT_PASSWORD}

volumes:

- ./event-server/init-event.js:/docker-entrypoint-initdb.d/init-event.js:ro

- mongo-event-data:/data/db

  

volumes:

mongo-auth-data:

mongo-event-data:
```