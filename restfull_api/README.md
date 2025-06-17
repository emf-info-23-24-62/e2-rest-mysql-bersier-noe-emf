# Fichiers de configuration
Ce répertoire contient les fichiers de configuration pour installer votre application.

- Dockerfile
```Dockerfile
FROM node:18
WORKDIR /app
RUN git clone https://github.com/almoggutin/node-express-rest-api-mysql-js-example
WORKDIR /app/node-express-rest-api-mysql-js-example
COPY .env ./
RUN npm i
CMD ["npm", "run", "dev"]

```

- docker-compose.yml
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
