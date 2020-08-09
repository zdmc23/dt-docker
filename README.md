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

2. `docker-compose up -d`

*That's it!*  

(Upon initial run, you need to step through the standard WordPress setup form (~5 fields), and then you can login and access D.T with pre-install Theme, plugins, any supporting files (e.g., `.htaccess`, `upload.ini`), etc...).

#### Restore from backup (optional)

Assumes a previous `mysqldump` (`docker exec -i ubuntu_db_1 mysqldump -uroot -p wordpress > 20200809.sql`).

- Edit `docker-compose.yml` file to include the following under the `db` -> `volumes` block:
  ```
    ...
    db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
       - ./20200809.sql:/20200809.sql
     restart: always
     environment:
     ...
  ```
- `docker exec -it ubuntu_db_1 bash`
- `mysql -uroot -p wordpress`
- `mysql>source 20200809.sql`
  
Note 1: These instructions assume that `ubuntu_db_1` is the name of the container running MySQL. To find the names of your runnning containers: `docker ps`
Note 2: You do not need to step through the WordPress onboarding to restore DB from `mysqldump` - just skip it.
Note 3: Depending on your setup, you most likely need to run the above Docker commands using **sudo** (e.g., `sudo docker ps`)

