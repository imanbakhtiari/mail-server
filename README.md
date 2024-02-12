## Mail-Server installation using docker

### just install docker and docker-compose
### then just create a docker-compose.yml file and paste this configuration in it

```bash
version: '3'
services:
  posteio:
    image: analogic/poste.io
    ports:
      - "25:25"
      - "80:80"
      - "443:443"
      - "110:110"
      - "143:143"
      - "465:465"
      - "587:587"
      - "993:993"
      - "995:995"
    environment:
      - TZ=Europe/Prague
    volumes:
      - /opt/mail/data:/data
    restart: always
```
### And then run docker compose up

```bash
docker-compose up -d 
```
