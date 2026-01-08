# 🐳 Instal·lació i desplegament de Mailcow

## Entorn utilitzat
- Sistema operatiu: Ubuntu (WSL2)
- Docker i Docker Compose instal·lats
- Repositori oficial de Mailcow

## Desplegament
El projecte es desplega utilitzant Docker Compose, aixecant tots els serveis necessaris:
- Postfix
- Dovecot
- MariaDB
- Redis
- Rspamd
- SOGo
- Nginx
- Watchdog

## Verificació
S’executa `docker-compose ps` per comprovar que **tots els contenidors estan en estat running/healthy**.
