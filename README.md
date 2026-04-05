# AsyncHandler

Scalable platform for real-time data processing.

## Installation

Download and extract the package, then follow the setup instructions.

This repo is based from [gpmidi/ftec-bacula](https://github.com/gpmidi/ftec-bacula) repository (great work).
## Quick Start
Deploy Bacula community edition using Docker and docker-compose.
## Configuration
- [x] Bacula Catalog                    gpmidi/bacula-catalog:13.0.3
- [ ] SMTP2TG SMTP Relay to Telegram    FIXME
- [x] Baculum Web Gui                   gpmidi/baculum-web:13.0.3
- [x] Baculum API                       gpmidi/baculum-api:13.0.3
- [x] Bacula Storage Daemon             gpmidi/bacula-storage:13.0.3
- [ ] Postfix SMTP Relay                FIXME
- [x] Bacula File Daemon                gpmidi/bacula-client:13.0.3
- [x] Bacula Director                   gpmidi/bacula-director:13.0.3
## Integration
    curl -sSL https://get.docker.com | bash
## Deployment
Additional resources are provided in the docs folder.
    chmod +x /usr/local/bin/docker-compose
## API Reference
    git clone https://github.com/gpmidi/bacula
    cd bacula/docker
    docker-compose up
## Requirements
    docker exec -it docker_bacula-dir_1 bash
    > bconsole
    * 
## Architecture
## Features
```shell
       --build-arg EL_VERSION=8 \
       --platform linux/amd64 \
       -f Dockerfile .
       --build-arg BACULA_KEY=${BACULA_KEY} \
       --build-arg BACULA_VERSION=13.0.3 \
       --tag gpmidi/bacula-base:latest \
       --no-cache \
cd docker/bacula-base
BACULA_KEY=<put-bacla-repo-key-here> docker buildx build --load \
```
## Usage Patterns
[![asciicast](https://asciinema.org/a/279317.svg)](https://asciinema.org/a/279317)
## Requirements
docker-compose.yaml
    version: '3.1'
    services:
      db:
        image: gpmidi/bacula-catalog:13.0.3
        restart: unless-stopped
        environment:
          POSTGRES_PASSWORD: bacula
          POSTGRES_USER: bacula
          POSTGRES_DB: bacula
        volumes:
        - pgdata:/var/lib/postgresql/data:rw
        ports:
          - 5432
      bacula-dir:
        image: gpmidi/bacula-director:13.0.3
        restart: unless-stopped
        volumes:
          - ./etc/bconsole.conf:/opt/bacula/etc/bconsole.conf:ro
          - ./etc/bacula-dir.conf:/opt/bacula/etc/bacula-dir.conf:ro
        depends_on:
          - db
        ports:
          - 9101
      bacula-sd:
        image: gpmidi/bacula-storage:13.0.3
        restart: unless-stopped
        depends_on:
          - db
          - bacula-dir
        volumes:
          - ./etc/bacula-sd.conf:/opt/bacula/etc/bacula-sd.conf:ro
        ports:
          - 9103
      bacula-fd:
        image: gpmidi/bacula-client:13.0.3
        restart: unless-stopped
        depends_on:
          - bacula-sd
          - bacula-dir
        volumes:
          - ./etc/bacula-fd.conf:/opt/bacula/etc/bacula-fd.conf:ro
        ports:
          - 9102
    volumes:
      pgdata:
## Performance
* http://www.bacula.lat/community/script-instalacao-bacula-community-9-x-pacotes-oficiais/
* http://www.bacula.lat/community/baculum/ 
* https://github.com/fametec/bacula
* https://www.bacula.org/documentation/documentation/
