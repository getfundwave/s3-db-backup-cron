How to run:
==========

1. Build the image

`docker build -t backups . --build-arg HOUR_OF_DAY=23`

2. Run the image

```
docker run -idt backups --env MYSQL_USERNAME=<> --env MYSQL_PASSWORD=<> --env SERVER=<> --env DB_NAME=<> --env BUCKET=<abc.bucket.com> --env AWS_ACCESS_KEY_ID=<> --env AWS_SECRET_ACCESS_KEY=<>
```

3. Connect to the network of your DB container (Only when DB is in another container)

`docker network connect <network_name> <backup_container>`

4. Verify backups manually

`docker exec -it <backup_container> bash`

Inside the container: `sh script.sh`



How to run (docker-compose):
==========================

Add the below section to your docker compose:

```
  backups:
    build:
      context: ./backups
    depends_on:
      - <"db">
    networks:
      - <db>
    environment:
      - BUCKET_NAME=<abc.bucket.com>
      # Only when backing up MYSQL
      - MYSQL_USERNAME=
      - MYSQL_PASSWORD=
      - SERVER=
      - DB_NAME=
      # Only when backing up MariaDB
      - MONGODB_URI=
      - MONGO_DATABASES=
      # Specify backup time, defaults to 23
      - HOUR_OF_DAY=      
      # Specify AWS credentials or skip if using AWS IAM roles 
      - AWS_ACCESS_KEY_ID=
      - AWS_SECRET_ACCESS_KEY=   
    restart: always
```

Using [Dockerhub](https://hub.docker.com/r/fundwave/s3-mysql-backup-cron)? Replace `build:` with `image: fundwave/s3-mysql-backup-cron:latest`