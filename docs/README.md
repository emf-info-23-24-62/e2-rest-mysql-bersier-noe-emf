# Documentation technique
Voici les commandes qui m'ont été utiles lors de ce test
Pour build les images et les container a partir du docker-compose 
```docker
docker-compose up -d
```
Pour redémarer le container afin qu'il fonctionne
```docker
docker restart e2-347-backend-1
```
Commandes pour supprimer toutes les images 
```docker
docker rmi -f $(docker images -q)
```
Commande pour supprimer tous les volumes
```docker
docker volume rm $(docker volume ls -q)
```
Commande pour supprimer tous les containers
```docker
docker rm -f $(docker ps -aq)
```
Voici ce qui apparait quand je me rend ici http://localhost:8080/api
![image](https://github.com/user-attachments/assets/912740d5-0607-4d00-9ba5-2f5fd861dde6)

Voici  tous mes fichiers

Dockerfile du backend 
```Dockerfile
#image node
FROM node:18
#chemin de travail
WORKDIR /app
#clone du projet 
RUN git clone https://github.com/almoggutin/node-express-rest-api-mysql-js-example
#chemin dans le clone
WORKDIR /app/node-express-rest-api-mysql-js-example
#copie du fichier .env
COPY .env ./
RUN npm i
CMD ["npm", "run", "dev"]

```

Docker-compose 
```yaml
services:
  db:
    image: mysql:5.7
    ports:
      - "3306:3306"
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpass347
      MYSQL_DATABASE: api_example
      MYSQL_USER: user347
      MYSQL_PASSWORD: pass347
    volumes:
      - mysql-data:/var/lib/mysql
      - ./db/init.sql:/docker-entrypoint-initdb.d/init.sql:ro
 
  backend:
    build: ./backend
    ports:
      - "8080:3000"
    restart: always

volumes:
  mysql-data:
 
```
et mon .env 
```.env
PORT=3000
DB_HOST=db
DB_PORT=3306
DB_USERNAME=user347
DB_USERNAME_PASSWORD=pass347
DB_NAME=api_example
```
