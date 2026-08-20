Glances is a free and open-source system monitoring tool that provides a web interface for monitoring CPU, memory, disk, network, processes, and Docker containers.

This guide shows how to run Glances with Docker Compose so that it automatically detects the host machine and its Docker containers.

## What You Will Get

After installation, Glances will provide a web dashboard showing:
- CPU usage
- Memory usage
- Swap
- Load average
- Disk usage
- Disk I/O
- Network traffic
- Running processes
- Docker containers
- Docker container resource usage
- Host operating system information

The Docker setup uses the host PID namespace and host networking so Glances can see the host system rather than only its own container. The Docker socket allows Glances to discover and monitor Docker containers automatically.

## Requirements

You need:
- Linux server
- Docker
- Docker Compose
- Access to port `61208`

Check Docker:

```bash
docker --version
```

Check Docker Compose:

```bash
docker compose version
```

If both commands work, you are ready to continue.

## 1. Create the Glances Directory

Create a directory for the Glances installation:

```bash
mkdir -p ~/glances
cd ~/glances
```

## 2. Create the Docker Compose File

Create the Compose file:

```bash
nano docker-compose.yml
```

Add the following configuration:

```yaml
services:
  glances:
    image: nicolargo/glances:latest-full
    container_name: glances
    restart: unless-stopped

    pid: host
    network_mode: host

    environment:
      GLANCES_OPT: "-w"

    volumes:
      # Host filesystem
      - /:/rootfs:ro

      # Docker monitoring
      - /var/run/docker.sock:/var/run/docker.sock:ro

      # Host operating system information
      - /etc/os-release:/etc/os-release:ro

    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:61208/api/4/status"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

The official Glances Docker Compose example uses the `latest-full` image, host PID/network mode, and the Docker socket. The current official example also uses a health check against the Glances API.

## 3. Start Glances

Run:

```bash
docker compose up -d
```

Docker will automatically download the Glances image if it is not already available.

Check the container:

```bash
docker compose ps
```

You should see something similar to:

```text
NAME       IMAGE                              STATUS
glances    nicolargo/glances:latest-full     Up
```

## 4. Open the Web Interface

Because Glances is running with host networking, its web server is available directly on port `61208`.

Open:

```text
http://SERVER_IP:61208
```

For example:

```text
http://192.168.1.100:61208
```

Glances uses port `61208` for its web interface/API.

## 5. Verify Host Monitoring

The important part of this setup is that Glances is not limited to monitoring its own Docker container.

The following configuration allows Glances to inspect the host:

```yaml
pid: host
network_mode: host
```

The root filesystem is also mounted:

```yaml
- /:/rootfs:ro
```

This allows Glances to access host filesystem information while keeping the mount read-only.

You should now see information for the actual server:

```text
CPU
Memory
Load
Disk
Network
Processes
```

## 6. Verify Docker Monitoring

The Docker socket is mounted with:

```yaml
- /var/run/docker.sock:/var/run/docker.sock:ro
```

This allows Glances to communicate with the Docker daemon and automatically discover containers. The mount is read-only from the container's perspective.

If you have containers such as:

```text
nginx
postgres
redis
app
```

Glances should automatically detect them.

You do not need to manually add each container.

## 7. Check the Logs

If the dashboard does not load, check the logs:

```bash
docker logs glances
```

Or:

```bash
docker compose logs -f glances
```

You can also check the container status:

```bash
docker inspect glances --format '{{.State.Status}}'
```

Expected output:

```text
running
```

## 8. Test the API

Glances provides an API that can be used to verify that the service is working.

Run:

```bash
curl http://localhost:61208/api/4/status
```

You should receive a JSON response.

The official Glances Compose configuration uses the same API endpoint for its health check.

## 9. Automatically Start After Reboot

The Compose configuration contains:

```yaml
restart: unless-stopped
```

This means Docker will automatically restart Glances after a server reboot, provided Docker itself starts normally.

You do not need to manually run:

```bash
docker compose up -d
```

after every reboot.

## 10. Updating Glances

To update to the latest image:

```bash
cd ~/glances
docker compose pull
docker compose up -d
```

Check the running version:

```bash
docker exec glances glances --version
```

The official project provides several Docker image tags. `latest-full` is the full-featured Alpine-based release image, while `latest` is a smaller image with fewer dependencies.

## 11. Stop Glances

To stop the monitoring service:

```bash
docker compose down
```

To start it again:

```bash
docker compose up -d
```

## 12. Protect the Web Interface

If port `61208` is exposed to the Internet, do not leave the Glances interface publicly accessible without protection.

Glances supports password protection. The official documentation provides Docker configurations using `--password` and a password file.

For a server exposed to the Internet, a reverse proxy such as Nginx, Caddy, or Traefik can also be used to provide HTTPS and authentication.

For a private LAN, you can instead restrict access to port `61208` with your firewall.

## 13. Firewall Example

If you use UFW and only want Glances accessible from your local network, you could allow your LAN subnet:

```bash
sudo ufw allow from 192.168.1.0/24 to any port 61208 proto tcp
```

Replace `192.168.1.0/24` with your actual LAN subnet.

Do not blindly open the port to the entire Internet with:

```bash
sudo ufw allow 61208/tcp
```

unless you have additional authentication and security controls in place.

## 14. Complete Installation

The complete installation can be reduced to:

```bash
mkdir -p ~/glances
cd ~/glances
nano docker-compose.yml
```

Paste:

```yaml
services:
  glances:
    image: nicolargo/glances:latest-full
    container_name: glances
    restart: unless-stopped

    pid: host
    network_mode: host

    environment:
      GLANCES_OPT: "-w"

    volumes:
      - /:/rootfs:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /etc/os-release:/etc/os-release:ro

    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:61208/api/4/status"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

Then run:

```bash
docker compose up -d
```

Finally open:

```text
http://SERVER_IP:61208
```

## Result
You now have a lightweight, self-hosted monitoring dashboard running entirely in Docker:
```text
                    Web Browser
                         |
                         |
                  :61208 / Glances
                         |
              +----------+----------+
              |                     |
              v                     v
        Host monitoring       Docker monitoring
              |                     |
       +------+------+       +------+------+------+
       |      |      |       |      |      |      |
      CPU    RAM    Disk    nginx  app   redis  db
       |      |      |       |      |      |      |
       +------+------+       +------+------+------+
```

There is no separate monitoring server, agent registration, API token, or manual system configuration required for this single-host Docker setup.
### Official References
- [Glances Docker documentation](https://glances.readthedocs.io/en/latest/docker.html)
- [Glances GitHub repository](https://github.com/nicolargo/glances)
- [Official Glances Docker Compose example](https://github.com/nicolargo/glances/blob/develop/docker-compose/docker-compose.yml)