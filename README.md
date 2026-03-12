### DockerHub
[`metabase/mariadb`](https://hub.docker.com/repository/docker/metabase/mariadb)
[![](https://images.microbadger.com/badges/version/metabase/mariadb.svg)](https://microbadger.com/images/metabase/mariadb)

A version of the official MariaDB Docker image that sets the default
[collation](https://mariadb.com/docs/server/server-management/variables-and-modes/server-system-variables#collation_server)
to `utf8mb4_unicode_ci`. (See [Slack thread](https://metaboat.slack.com/archives/CKZEMT1MJ/p1773319838474849) for more
details).

You can verify this by running locally after starting the daemon and CLI.

```sql
SELECT * FROM INFORMATION_SCHEMA.SCHEMATA;
```

```
+--------------+--------------------+----------------------------+------------------------+----------+----------------+
| CATALOG_NAME | SCHEMA_NAME        | DEFAULT_CHARACTER_SET_NAME | DEFAULT_COLLATION_NAME | SQL_PATH | SCHEMA_COMMENT |
+--------------+--------------------+----------------------------+------------------------+----------+----------------+
| def          | mb_test            | utf8mb4                    | utf8mb4_unicode_ci     | NULL     |                |
+--------------+--------------------+----------------------------+------------------------+----------+----------------+
```

At the time of this writing only done for 10.6 since `latest` seems to work without doing this, but we may need to
publish more images in the future.

### Build It

```bash
cd 10.6
docker build -t metabase/mariadb:10.6 .
```

### Run Locally (Daemon)

```bash
docker run \
  --network mariadb \
  -p 3306:3306 \
  -e MARIADB_ALLOW_EMPTY_ROOT_PASSWORD=true \
  -e MARIADB_DATABASE=mb_test \
  -it metabase/mariadb:10.6
```

### Run Locally (CLI)

# Get the IP address of the daemon from `docker network inspect mariadb`.

```bash
docker run \
  --network mariadb \
  -it metabase/mariadb:10.6 \
  mariadb \
  --host 172.18.0.2 \
  --port 3306 \
  --user root \
  --database mb_test
```

### Push It

```bash
cd 10.6
docker push metabase/mariadb:10.6
```
