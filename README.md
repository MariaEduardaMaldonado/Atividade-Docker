# WordPress + MYSQL com o Docker Compose 

O Compose é uma ferramenta do Docker usada para executar aplicativos de vários containers. Com um comando, através do arquivo YAML é possível criar todos os serviços de configuração: services, volumes e a rede do aplicativo. 

### 📋 Pré-requisitos
> Ter o Docker Desktop e Docker Compose instalados em seu computador.

### 🔧 Instalação
Docker Desktop

> https://docs.docker.com/engine/install/

Docker Compose
> **Observação:** No Docker Desktop o docker-compose já vem instalado.

1. Primeiramente, crie um diretório para o seu projeto. 
```
$ mkdir Wordpress 
```
2. Mude para o seu diretório 
```
$ cd ./Wordpress 
```
3. Crie o arquivo docker-compose.yml e abra em um editor de texto.
```
$ New-Item docker-compose.yml 
```

Vamos começar a montar o nosso arquivo `docker-compose.yml`.

## 🛠️ Construção do arquivo Docker Compose

1. Primeiro, vamos especificar a versão do arquivo .yaml. Vamos usar a versão mais atual com suporte.
```ruby
version: '3.8'
```
2. Agora, vamos definir os serviços para a aplicação.
```
Services:
```
2.1 Definindo as configurações do mysql
```ruby
  mysql:
    image: mysql:8.0.31
    restart: always
    command: mysqld --default_authentication_plugin=mysql_native_password
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: docker
      MYSQL_USER: docker
      MYSQL_PASSWORD: docker
      MYSQL_DATABASE: wordpress
    ports:
      - 3306:3306
    networks:
      - wordpress-network
```
2.2 Definindo as configurações do phpMyAdmin (opcional)
> **Observação:** Essa parte é opcional, mas serve para a administração do MYSQL pelo navegador.
```ruby
  phpmyadmin:
    depends_on:
      - mysql
    image: phpmyadmin:5.2.0
    restart: always
    ports:
      - 8080:80
    environment:
      PMA_HOST: mysql
      MYSQL_ROOT_PASSWORD: docker
    networks:
      - wordpress-network
```
2.3 Definido as configurações do Wordpress
```ruby
  wordpress:
    image: wordpress:6.0.2
    restart: always
    volumes:
      - wp_data:/var/www/html
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: docker
    ports:
      - 80:80
    networks:
      - wordpress-network
```
3. Definindo as configurações de rede
```ruby
networks:
  wordpress-network:
    driver: bridge
```
4. E por fim, criaremos os volumes
```ruby
volumes:
  db_data:
  wp_data:
```
📃
O arquivo final se encontra neste link: https://github.com/MariaEduardaMaldonado/Atividade-Docker/blob/main/docker-compose.yml
