# Docker Compose Configuration for Squid Proxy

This Docker Compose configuration sets up a Squid proxy service using Alpine Linux as the base image. It is specifically designed to facilitate connections to GitHub Copilot while blocking connections to JetBrains.com, ensuring focused usage and controlled network traffic.

## Services

### copilot-proxy

- **Image**: `alpine:latest` - Uses the latest version of the Alpine Linux image.
- **Container Name**: `copilot-proxy` - The name assigned to the running container.
- **Command**: The container executes several commands at startup:
  - `apk update` - Updates the package index.
  - `apk add apache2-utils` - Installs Apache utilities.
  - `apk add curl` - Installs Curl for making HTTP requests.
  - `apk add squid` - Installs Squid proxy.
  - `rm -f /var/run/squid.pid` - Removes the Squid PID file if it exists to allow Squid to restart.
  - `squid -N` - Runs Squid in non-daemon mode.
- **Ports**:
  - `3128:3128` - Maps port 3128 on the host to port 3128 in the container, where Squid listens for incoming connections.
- **Volumes**:
  - `./squid.conf:/etc/squid/squid.conf:ro` - Mounts the Squid configuration file from the host to the container as read-only.
- **Environment Variables**:
  - `SQUID_ENABLE_IPV6=true` - Enables IPv6 support in Squid.

## Networks

### copilot-net

- **Driver**: `bridge` - Uses the Docker bridge driver to create a network bridge allowing containers connected to the same bridge to communicate.

## Usage

To start the proxy service, navigate to the directory containing this `docker-compose.yml` and run:

```bash
docker-compose up -d
```
