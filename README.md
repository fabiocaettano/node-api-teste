# Node App Base

Índice
* [Aplicação Node](#aplicação-node)
* [Dockerfile](#dockerfile)
* [Construindo a imagem com Docker](#Construindo-a-imagem-com-Docker)
* [Docker-Compose](#Docker-Compose)
* [Git](#git)



# Aplicação Node

Criar o arquivo package.json:

``` cli
npm init
```

Configurar o package.json, incluir a dependência express:

``` json
{
  "name": "nome da aplicação",
  "version": "1.0.0",
  "description": "descrição do projeto",
  "author": "Nome <nome@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}
```

Instalar a dependência express, se o npm tiver a versão maior ou igual 5 será criado o arquivo package-lock.json:

```
npm install
```

Criar o arquivo server.js:

``` javascript
'use strict';

const express = require('express');

const PORT = 8080;
const HOST = '0.0.0.0';

const app = express();
app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```

# Dockerfile

Criar o Dockerfile:

``` dockerfile
FROM node:16

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 8080

CMD ["npm","start"]
```

Criar o arquivo .dockerignore:

```
node_modules
```

# Construindo a imagem com Docker


Criar a imagem:

``` cli
docker build -t fabiocaettano74/api-teste:01 .
```

Executar o container:

``` cli
docker run -p 8080:8080 -d fabiocaettano74/api-teste:01
```

Verificar logs:

```
docker logs idContainer
```


Acessar o container:

``` cli 
docker exec -it idContainer /bin/bash
```

Visualizar o app via http:
 
 ``` html
 http://localhost:8080
 ```

Visualiar o app via curl: 

``` curl
curl -i http://localhost:8080
```

# Docker Compose

Deletar o container: 

```
docker container ls
docker container rm -f idDoContainer
```

Criar o manifesto:

``` yaml
version: '3.3'
services:
  node:
    build:
      context: node
    command: bash -c "npm start"
    ports:
      - "8080:8080"
    networks:
      - node-network
networks:
  node-network:
    driver: bridge
```

Criar o container:

```
docker-compose up -d
docker container ls
```

Excluir
```
docker-compose down
```


# Git

Criar o arquivo .gitignore:

```
node_modules/
```

Comandos:

``` git
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/fabiocaettano/node-api-teste.git
git push -u origin master
```

Referências:

! [Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)