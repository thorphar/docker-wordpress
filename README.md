# Template Docker Wordpress
This is suited to running on a raspberry pi, as such a different database image will be used.

## Docker images

- linuxserver/mariadb
- wordpress:latest

*specific version were the following*

|   image   |   version |
|   ------- | --------- |
|   linuxserver/mariadb |   110.4.19mariabionic-ls22    | 
|   wordpress           |   latest as of 09/10/21       | 


## Docker container configurations

### LinuxServer / mariadb

docker pull linuxserver/mariadb:110.4.19mariabionic-ls22


|   Variable   |   Value  |
|   --------    | -------- |
|   MYSQL_ROOT_PASSWORD |   wp_password   |
|   MYSQL_DATABASE  |   wp_db   |  
|   MYSQL_USER  |   wp_user   |
|   MYSQL_PASSWORD  |   wp_password   |
|   MYSQL_DIR  |   /config   |
|   DATADIR  |   /config/database   |

**Volume Mapping**
```
container = /config
-> volume = (Select previously create volume)
```

**Port Publishing**

```
host:[3308]
container:[3306] (This is the default of mariadb)
```

### Wordpress 

|   Variable   |   Value  |
|   --------    | -------- |
|   WORDPRESS_DB_HOST |   ip_of_above_container:port[3308]   |
|   WORDPRESS_DB_USER  |   same as defined above [wp_user]   |  
|   WORDPRESS_DB_PASSWORD  |   same as defined above [wp_password]   |
|   WORDPRESS_DB_NAME  |   same as defined above [wp_db]   |

**Volume Mapping**
```
container = /var/www/html
-> volume = (Select previously create volume)
```

**Port Publishing**

```
host:[8092]
container:[80] (This is the default of mariadb)
```

## Setup Process

* Create named volume for wordpress
* Create named volume for DB
* Create DB container
* Create wordpress container
* Set up wordpress through the web browser (http://local_address:8092)
* Create user and enter the settings and prepare to change the website address to be changed.
* Go to your DDNS (if you are using one otherwise you domain manager) service, I add an A record to include subdomains if testing otherwise set up the records to direct traffic to your intented target.
* I use nginx proxy manager to provide a reverse proxy service as well as a SSL manager. 
* Once these last two steps are complete then the wordpress settings can be adjusted to accept entry through the intended domain. The address needs to updated in WordPress Address (URL) and Site Address (URL) (You will need to include the https:// element of the address) 
* You will lose access through the local_address and will need to now use the domain to access the website manager.
