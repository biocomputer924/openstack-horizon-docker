# openstack-horizon-docker

## Build
`docker build -t horizon .`

## Run Horizon via docker-compose
Example docker-compose.yml for Horizon:

```
version: "2"
services:
  horizon:
    image: horizon
    environment:
      OPENSTACK_API_VERSIONS: "{\"identity\": 3, \"image\": 2, \"volume\": 2,}"
      OPENSTACK_HOST: "\"keystone\""
      OPENSTACK_KEYSTONE_DEFAULT_ROLE: "\"user\""
      OPENSTACK_KEYSTONE_URL: "\"http://keystone:5000/v3\""
    ports:
      - 80

  keystone:
    image: yotsuka/keystone
    environment:
      OS_BOOTSTRAP_USRNAME: admin
      OS_BOOTSTRAP_PASSWORD: password
      OS_BOOTSTRAP_SERVICE_NAME: keystone
      OS_BOOTSTRAP_REGION_ID: RegionOne
      OS_BOOTSTRAP_ADMIN_URL: http://keystone:5000/v3
      OS_BOOTSTRAP_PUBLIC_URL: http://keystone:5000/v3
      OS_BOOTSTRAP_INTERNAL_URL: http://keystone:5000/v3
      KEYSTONE_DATABASE_CONNECTION: mysql://root:@keystone-database/keystone
      KEYSTONE_CACHE_HOST: keystone-cache:11211
    ports:
      - 5000
    restart: always

  keystone-cache:
    image: memcached

  keystone-database:
    image: mysql
    environment:
      MYSQL_ALLOW_EMPTY_PASSWORD: "yes"
      MYSQL_DATABASE: keystone
```
