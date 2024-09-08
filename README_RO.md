<p align="center">
  <img src="https://www.virtualizationhowto.com/wp-content/uploads/2023/10/Nginx-Proxy-Manager-Docker-Install.png" alt="Instalare Nginx Proxy Manager Docker" />
</p>

# Configurare Docker Compose pentru NGINX Proxy Manager

Această configurare setează NGINX Proxy Manager folosind Docker Compose, inclusiv variabilele de mediu necesare și mapările volumelor. De asemenea, arată cum să conectați NGINX Proxy Manager cu o bază de date MariaDB.

## Fișier Docker Compose

```yaml
version: '3.8'

services:
  nginx-proxy-manager:
    image: 'jc21/nginx-proxy-manager:latest'
    container_name: NGINX-proxy-manager
    restart: unless-stopped
    ports:
      - '80:80'         # Port HTTP
      - '443:443'       # Port HTTPS
      - '81:81'         # Port Admin
    environment:
      DB_MYSQL_HOST: database       # Numele containerului sau aliasul de rețea al containerului MariaDB
      DB_MYSQL_PORT: 3306           # Portul implicit MariaDB
      DB_MYSQL_USER: <DB_USER>      # Utilizatorul bazei de date pentru NGINX Proxy Manager
      DB_MYSQL_PASSWORD: <DB_PASS>  # Parola pentru utilizatorul bazei de date
      DB_MYSQL_NAME: <DB_NAME>      # Numele bazei de date utilizate de NGINX Proxy Manager
    volumes:
      - '/path/to/nginx-proxy-manager/data:/data'          # Stocare persistentă pentru datele NGINX Proxy Manager
      - '/path/to/nginx-proxy-manager/letsencrypt:/etc/letsencrypt'  # Stocare pentru certificate Let's Encrypt
    networks:
      - webnet-network

networks:
  webnet-network:
    external: true
```
## Explicație

### Variabile de Mediu

- **`DB_MYSQL_HOST`**: Setează aceasta la numele containerului sau aliasul de rețea al serviciului MariaDB (de exemplu, `database`). Acest lucru permite NGINX Proxy Manager să se conecteze la instanța MariaDB.
- **`DB_MYSQL_PORT`**: Portul implicit pentru MariaDB (`3306`).
- **`DB_MYSQL_USER`**: Contul de utilizator pe care NGINX Proxy Manager îl va folosi pentru a se conecta la baza de date MariaDB. Înlocuiește `<DB_USER>` cu numele de utilizator real.
- **`DB_MYSQL_PASSWORD`**: Parola pentru contul `DB_MYSQL_USER`. Înlocuiește `<DB_PASS>` cu parola reală.
- **`DB_MYSQL_NAME`**: Numele bazei de date MariaDB pe care NGINX Proxy Manager o va folosi. Înlocuiește `<DB_NAME>` cu numele real al bazei de date.

### Volume

- **`/path/to/nginx-proxy-manager/data:/data`**: Acest path pe gazdă este folosit pentru a stoca datele persistente pentru NGINX Proxy Manager. Ajustează `/path/to/nginx-proxy-manager/data` la directorul corespunzător de pe sistemul tău.
- **`/path/to/nginx-proxy-manager/letsencrypt:/etc/letsencrypt`**: Acest path pe gazdă este folosit pentru a stoca certificatele Let's Encrypt. Ajustează `/path/to/nginx-proxy-manager/letsencrypt` la directorul corespunzător de pe sistemul tău.

### Rețea

- **`webnet-network`**: Această rețea conectează containerul NGINX Proxy Manager la containerul MariaDB. Asigură-te că ambele servicii sunt pe aceeași rețea pentru a permite comunicarea corespunzătoare.

## Rezumat

- **Configurarea Rețelei**: Atât NGINX Proxy Manager, cât și MariaDB trebuie să fie pe aceeași rețea Docker (`webnet-network`).
- **Variabile de Mediu**: Configurează corect detaliile conexiunii la baza de date pentru a asigura că NGINX Proxy Manager se poate conecta la MariaDB.
- **Volume**: Asigură-te că căile gazdei pentru date și certificate sunt configurate corect pentru persistență și securitate.

Această configurare va permite NGINX Proxy Manager să funcționeze eficient cu MariaDB, asigurând în același timp persistența datelor și gestionarea certificatelor.
