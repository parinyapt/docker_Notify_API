# Run Notify API with Docker Compose

## Getting Started
#### Step 1: Add Environment Variable on .env file
```env
CONTAINER_NAME=
IMAGE_TAG=v1.0
HOST_ALIAS_NAME=
JSON_CONFIG_FILE_PATH=./config/api_config.json
NETWORK_NAME=
```
- HOST_ALIAS_NAME will use as request URL from another service

#### Step 2: Create Docker Network
```bash
docker network create {NETWORK_NAME}
```

#### Step 3: Set Service Environment Variable in docker-compose.yml
```yml
TZ: Asia/Bangkok
```
note: TZ is timezone that refer to [List of tz time zones](https://go.dev/src/time/zoneinfo_abbrs_windows.go)

#### Step 4: Add API Config file
```json
{
  "service": {
    "SERVICE_ID": {
      "name": "SERVICE_NAME",
      "auth": {
        "access_key": "SERVICE_ACCESS_KEY"
      },
      // Optional - If not set, will not send notification to LINE
      "line": {
        "notify_token": "LINE_NOTIFY_TOKEN"
      },
      // Optional - If not set, will not send notification to Discord
      "discord": {
        "webhook_id": "DISCORD_WEBHOOK_ID",
        "webhook_token": "DISCORD_WEBHOOK_TOKEN"
      },
      // Optional - If not set, will not send notification to Email
      "mail": {
        "smtp_server": "SMTP_SERVER", // ex. smtp.gmail.com
        "smtp_port": "SMTP_PORT", // ex. 465
        "smtp_username": "SMTP_USERNAME",
        "smtp_password": "SMTP_PASSWORD",
        "sender_alias_name": "SENDER_ALIAS_NAME",
        "sender_email": "SENDER_EMAIL",
        "sender_subject": "SENDER_SUBJECT",
        "receiver_email": ["mail@example.com", "mail2@example.com"]
      }
    },
    "SERVICE_ID_2": {
      ...
    },
  }
}
```

#### Step 5: Run Docker Compose
```bash
docker-compose up -d
```

#### Step 6: Test API from another service
```bash
curl -X POST http://{HOST_ALIAS_NAME}/{SERVICE_ID}/v1/notify \
-H 'AccessKey: {SERVICE_ACCESS_KEY}' \
-H 'Content-Type: application/json' \
-d '{"message":"Test API from another service"}'
```

#### Optional: Use Notify API public with Host Port
- Change expose and network in docker-compose.yml to this code
```yml
ports:
  - "${SERVICE_PORT}:80"
networks:
  - default
```

> By Parinya Termkasipanich