# MongoDB Docker Stack

Stack Docker Compose completo per eseguire MongoDB e Mongo Express in ambiente locale o development.

Il progetto include:

- MongoDB 7
- Mongo Express GUI
- configurazione tramite variabili `.env`
- persistenza dati
- healthcheck
- script di inizializzazione
- rete Docker dedicata

---

# Stack Architecture

```text
Mongo Express GUI
        ↓
MongoDB Container
        ↓
Persistent Volumes
```

---

# Project Structure

```text
mongodb-stack/
│
├── docker-compose.yml
├── .env
├── .gitignore
├── README.md
├── LICENSE
│
├── data/
├── configdb/
├── backups/
│
└── init/
    └── init.js
```

---

# Requirements

È necessario avere già installato:

- Docker Desktop
- Docker Compose

Verifica:

```bash
docker --version
docker compose version
```

---

# Installation

## 1. Clone repository

```bash
git clone <repository-url>
```

---

## 2. Enter project folder

```bash
cd mongodb-stack
```

---

## 3. Create `.env`

Creare il file `.env` partendo dal template fornito.

Esempio:

```env
MONGO_ROOT_USERNAME=admin
MONGO_ROOT_PASSWORD=super_secure_password
```

---

## 4. Start containers

```bash
docker compose up -d
```

---

## 5. Verify containers

```bash
docker ps
```

Dovrebbero comparire:

- mongodb
- mongo-express

---

# MongoDB Connection

## Local connection URI

```text
mongodb://admin:super_secure_password@localhost:27017
```

---

# Mongo Express GUI

## URL

```text
http://localhost:8081
```

## Login

Le credenziali sono definite nel file `.env`.

---

# Stop Containers

```bash
docker compose down
```

---

# Persistent Volumes

I dati vengono salvati localmente nelle directory:

```text
/data
/configdb
```

Questo garantisce la persistenza dei dati anche dopo il riavvio dei container.

---

# Init Scripts

Gli script presenti in:

```text
/init
```

vengono eseguiti automaticamente all'avvio iniziale del database.

Esempio:

```javascript
db.createUser({
  user: "appuser",
  pwd: "password",
  roles: [
    {
      role: "readWrite",
      db: "appdb"
    }
  ]
});
```

---

# Environment Variables

## MongoDB

| Variable | Description | Default |
|---|---|---|
| MONGO_IMAGE | MongoDB image version | mongo:7 |
| MONGO_CONTAINER_NAME | Container name | mongodb |
| MONGO_PORT | Exposed MongoDB port | 27017 |
| MONGO_ROOT_USERNAME | Root username | admin |
| MONGO_ROOT_PASSWORD | Root password | password123 |
| MONGO_INITDB_DATABASE | Initial database | appdb |
| MONGO_RESTART_POLICY | Restart policy | unless-stopped |

---

## Mongo Express

| Variable | Description | Default |
|---|---|---|
| MONGO_EXPRESS_IMAGE | Mongo Express image | mongo-express:latest |
| MONGO_EXPRESS_CONTAINER_NAME | Container name | mongo-express |
| MONGO_EXPRESS_PORT | GUI exposed port | 8081 |
| MONGO_EXPRESS_USERNAME | GUI username | express |
| MONGO_EXPRESS_PASSWORD | GUI password | express123 |
| MONGO_EXPRESS_RESTART_POLICY | Restart policy | unless-stopped |

---

## Timezone

| Variable | Description | Default |
|---|---|---|
| TZ | Container timezone | Europe/Rome |

---

# Useful Commands

## View logs

```bash
docker compose logs -f
```

---

## Restart stack

```bash
docker compose restart
```

---

## Access Mongo shell

```bash
docker exec -it mongodb mongosh -u admin -p super_secure_password
```

---

## Remove stack completely

```bash
docker compose down -v
```

⚠️ WARNING:

Questo comando elimina anche i volumi persistenti.

---

# Security Notes

Per ambienti production:

- usare password robuste
- usare TLS
- limitare esposizione porte
- usare Docker secrets
- evitare utenti root applicativi

---

# License

Distribuito sotto licenza MIT.

Vedi il file [LICENSE](LICENSE).