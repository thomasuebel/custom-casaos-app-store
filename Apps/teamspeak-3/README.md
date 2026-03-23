# TeamSpeak 3 + ts3-manager

A Docker Compose setup for a self-hosted TeamSpeak 3 server with the [ts3-manager](https://github.com/joni1802/ts3-manager) web interface. Compatible with CasaOS, ZimaOS, and other Docker app stores via the included `app.json`.

## Services

| Service | Image | Description |
|---|---|---|
| `teamspeak-server` | `teamspeak:latest` | TeamSpeak 3 voice server |
| `teamspeak-manager` | `joni1802/ts3-manager:latest` | Web-based administration UI |

## Ports

| Port | Protocol | Description |
|---|---|---|
| `9987` | UDP | Voice (connect clients here) |
| `10011` | TCP | ServerQuery (raw TCP) |
| `30033` | TCP | File transfer |
| `8080` | TCP | ts3-manager web UI |

## Getting Started

### 1. Accept the license

TeamSpeak requires explicit license acceptance. The `TS3SERVER_LICENSE_AGREEMENT=accept` environment variable in `docker-compose.yml` handles this. By deploying this stack you agree to the [TeamSpeak License Agreement](https://www.teamspeak.com/en/teamspeak3-server-license-agreement/).

### 2. Start the stack

```bash
docker compose up -d
```

### 3. Get the ServerQuery admin token

On first start, TeamSpeak generates a one-time admin token. Retrieve it from the logs:

```bash
docker logs teamspeak-server 2>&1 | grep token
```

### 4. Connect ts3-manager

Open `http://<your-host>:8080` in your browser and connect to the TeamSpeak server using:

- **Host:** `teamspeak-server` (internal Docker service name)
- **Port:** `10011`
- **Username:** `serveradmin`
- **Password/Token:** from step 3

### 5. Connect a TeamSpeak client

Use your host IP or domain and port `9987` (UDP) in your TeamSpeak client.

## Data

Server data (database, logs, uploaded files) is persisted in the `ts3server_data` Docker volume.

## License

MIT — see [LICENSE](LICENSE).

TeamSpeak is a product of TeamSpeak Systems GmbH and subject to its own [license agreement](https://www.teamspeak.com/en/teamspeak3-server-license-agreement/). ts3-manager is MIT licensed by Jonathan Francke.
