<p align="center">
  <img src="https://www.virtualizationhowto.com/wp-content/uploads/2023/10/Nginx-Proxy-Manager-Docker-Install.png" alt="Nginx Proxy Manager Docker Install" />
</p>

# Docker Compose Configuration for NGINX Proxy Manager

This configuration sets up NGINX Proxy Manager with Docker Compose, including the necessary environment variables and volume mappings. It also shows how to connect NGINX Proxy Manager with a MariaDB database.

## Docker Compose File

```yaml
version: '3.8'

services:
  nginx-proxy-manager:
    image: 'jc21/nginx-proxy-manager:latest'
    container_name: NGINX-proxy-manager
    restart: unless-stopped
    ports:
      - '80:80'         # HTTP port
      - '443:443'       # HTTPS port
      - '81:81'         # Admin port
    environment:
      DB_MYSQL_HOST: database       # Hostname or network alias of the MariaDB container
      DB_MYSQL_PORT: 3306           # Default MariaDB port
      DB_MYSQL_USER: <DB_USER>      # Database user for NGINX Proxy Manager
      DB_MYSQL_PASSWORD: <DB_PASS>  # Password for the database user
      DB_MYSQL_NAME: <DB_NAME>      # Database name used by NGINX Proxy Manager
    volumes:
      - '/path/to/nginx-proxy-manager/data:/data'          # Persistent storage for NGINX Proxy Manager data
      - '/path/to/nginx-proxy-manager/letsencrypt:/etc/letsencrypt'  # Storage for Let's Encrypt certificates
    networks:
      - webnet-network

networks:
  webnet-network:
    external: true
```
## Explanation

### Environment Variables

- **`DB_MYSQL_HOST`**: Set this to the container name or network alias of the MariaDB service (e.g., `database`). This allows NGINX Proxy Manager to connect to the MariaDB instance.
- **`DB_MYSQL_PORT`**: The default port for MariaDB (`3306`).
- **`DB_MYSQL_USER`**: The user account that NGINX Proxy Manager will use to connect to the MariaDB database. Replace `<DB_USER>` with the actual username.
- **`DB_MYSQL_PASSWORD`**: The password for the `DB_MYSQL_USER` account. Replace `<DB_PASS>` with the actual password.
- **`DB_MYSQL_NAME`**: The name of the MariaDB database that NGINX Proxy Manager will use. Replace `<DB_NAME>` with the actual database name.

### Volumes

- **`/path/to/nginx-proxy-manager/data:/data`**: This path on the host is used to store persistent data for NGINX Proxy Manager. Adjust `/path/to/nginx-proxy-manager/data` to the appropriate directory on your host system.
- **`/path/to/nginx-proxy-manager/letsencrypt:/etc/letsencrypt`**: This path on the host is used to store Let's Encrypt certificates. Adjust `/path/to/nginx-proxy-manager/letsencrypt` to the appropriate directory on your host system.

### Networking

- **`webnet-network`**: This network connects the NGINX Proxy Manager container to the MariaDB container. Ensure that both services are on the same network to allow proper communication.

## Summary

- **Network Configuration**: Both NGINX Proxy Manager and MariaDB must be on the same Docker network (`webnet-network`).
- **Environment Variables**: Properly configure the database connection details to ensure NGINX Proxy Manager can connect to MariaDB.
- **Volumes**: Ensure host paths for data and certificates are correctly set up for persistence and security.

This setup will allow NGINX Proxy Manager to operate with MariaDB effectively while ensuring data persistence and certificate management.

