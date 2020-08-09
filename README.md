# dt-docker
D.T deployment via Docker and Compose

#### Run

1. Create a `.env` file and copy/paste the following, and then update the values:

```
DOMAIN=mydomain
EMAIL=noreply@mydomain
MYSQL_USER=wordpress
MYSQL_PASSWORD=wordpress
MYSQL_ROOT_PASSWORD=somewordpress
```

(`DOMAIN` value can be an IP address - e.g., `127.0.0.1` or `192.168.1.2`)

2. `sudo docker-compose up -d`

*That's it!*  

(Upon initial run, you need to step through the standard WordPress setup form (~5 fields), and then you can login and access D.T with pre-install Theme, plugins, any supporting files (e.g., `.htaccess`, `upload.ini`), etc...).

#### Restore from backup (optional)

Assumes a previous `mysqldump` (`sudo docker exec -i ubuntu_db_1 mysqldump -uroot -p wordpress > 20200809.sql`).

- Edit `docker-compose.yml` file to include the following under the `db` -> `volumes` block:
  - `- ./20200809.sql:/20200809.sql`
- sudo docker exec -it [CONTAINER NAME OR ID - e.g., ubuntu_db_1] bash
- mysql -uroot -p wordpress
- mysql>source 20200809.sql
  
Note: you do not need to step through the WordPress onboarding to restore DB from `mysqldump` - just skip it.
