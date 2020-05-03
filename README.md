# Docker Nextcloud
Image with Nextcloud, Postgresql, Nginx, self-signed SSL certificate.

# Pre-requisites
## External drive
The setup assumed the usage of an external drive mounted as /media/ncdata/data, please adjust your setup accordingly or remove this etry from the docker-compose.yml, if you prefer not to use this feature.

```bash
sudo mount /dev/sda1 /media/ncdata
sudo chown -R www-data:www-data /media/ncdata/data

# check if the disk is mounted properly
mountpoint -q /media/ncdata && echo "mounted" || echo "not mounted"
```

## Add permission to the folder from within the container
```bash
docker exec -it docker-nextcloud_app_1 sh

chown -R www-data:root ./data
```

## DB connection
The setup uses an image of PostgreSQL as DB, please adjust the db.env and nc.env files with the (same) DB password.

# Add trusted domain from within the container
In case the external env variable do not work for some reason (it happened to me...), here how to do it from inside the container:

https://docs.nextcloud.com/server/latest/admin_manual/installation/installation_wizard.html#trusted-domains

# Create a self-signed SSL certificate
Indeed feel free to change the name of the certificate files, but do adjust the nginx.conf file and the web/Dockerfile in this case.

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout ./web/cloud.local.key -out ./web/cloud.local.crt
```

# Run!
```bash
# start
docker-compose up -d

# stop
docker-compose down

# stop and remove volumes (be careful here!)
docker-compose down -v
```

# References
* https://github.com/nextcloud/docker
* https://github.com/nextcloud/docker/tree/master/.examples/docker-compose/with-nginx-proxy/postgres/fpm
* https://github.com/docker/awesome-compose/blob/master/nextcloud-redis-mariadb/docker-compose.yaml
* https://www.docker.com/blog/awesome-compose-app-samples-for-project-dev-kickoff/
* https://www.rs-online.com/designspark/raspberry-pi-4-personal-datacentre-part-1-ansible-docker-and-nextcloud
* https://www.johnmackenzie.co.uk/post/creating-self-signed-ssl-certificates-for-docker-and-nginx/