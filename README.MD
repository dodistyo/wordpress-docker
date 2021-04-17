How to run :
1. make sure you have docker and docker compose installed
2. run command "docker-compose up -d"

Scaling : 
Nginx will act as a load balancer between wordpress server instance
Usually in kubernetes we can use replication but in this case (docker compose) we create and use multiple wordpress instances to scale the application and then the nginx will do the load balancing

How to Scale :
1. Duplicate wordpress instance in docker-compose file
   for example, you can add new instance 
---------------------------------------------------
   wordpress3:
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    deploy:
      resources:
        limits:
          cpus: '1'
          memory: 512M
    volumes:
      - wordpress:/var/www/html
---------------------------------------------------

2. And then add nginx configuration

---------------------------------------------------
upstream app_servers {

        server wordpress1:80;
        server wordpress2:80;
        server wordpress3:80;

    }
---------------------------------------------------

3. After that you need to build/rebuild the image with command "docker-compose build"

4. and then you can run command "docker-compose up -d" again
