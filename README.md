<h1 align="center">
Docker-NextCloud-Gitea
</h1>

# Table of Content
1. [About](#About)
2. [Setup](#Setup)
3. [Sources](#Sources)
# About
<p align="center">
    <img width="400" src="https://external-content.duckduckgo.com/iu/?u=http%3A%2F%2Flinoxide.com%2Fwp-content%2Fuploads%2F2017%2F01%2Fnginx-reverse-proxy-inside-docker.png&f=1&nofb=1"></img>
</p>
Dockerized NextCloud and Gitea behind a nginx reverse proxy with LetsEncrypt.

# Setup
1. Clone this repository:
    ```bash
    ~$ git clone https://gitea.ddnss.de/dvdvgt/Docker-NextCloud-Gitea.git
    ~$ cd Docker-NextCloud-Gitea
    ```
2. Create the neccassary environment files:
    ```bash
    ~$ touch nextcloud_app.env nextcloud_db.env gitea_app.env gitea_db.env
    ```
    2.1 For *_db.env files add the following:
    ```bash
    MYSQL_DATABASE=CHANGEME
    MYSQL_USER=CHANGEME
    MYSQL_PASSWORD=CHANGEME
    MYSQL_ROOT_PASSWORD=CHANGEME
    ```
    2.2 For *_app.env files add the following:
    ```bash
    VIRTUAL_HOST=my.domain.com
    LETSENCRYPT_HOST=my.domain.com
    LETSENCRYPT_EMAIL=my@mail.com
    ```
3. Create the required directories:
    ```text
    ~# mkdir /media/storage/nextcloud
    ~# mkdir /media/storage/gitea
    ```
    3.1 Change to ownership of the directories and all files in it:
    ```text
    ~# chown -R www-data:www-data /media/storage/nextcloud
    ~# chown -R www-data:www-data /media/storage/gitea
    ```
4. Create new virtual docker network:
    ```text
    ~# docker network create nginx-proxy
    ```
5. Start up the containers in detached mode. Should you encounter any issue it might be a good idea to run it normally and not in detached mode to figure out the problem.
    ```text
    ~# docker-compose up -d
    ```
    
Now that (hopefully) everything is running and has started up successfully you may now open NextCloud or Gitea via the virtual host your provided in the `.env` files.

When first opening either Gitea or NextCloud you have to do the final setup. Select the MySQL database for both Gitea and Nextcloud and enter the credentials from the *_db.env files (not the root password though).

Now wait until it's done setting up and that's it. Enjoy!

# Sources
The following software and images are used in this setup:

| Name | URL | Description |
| -------- | -------- | -------- |
| Docker   | https://docs.docker.com/install/ | Follow the instructions for installation. Note the [convience script](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-convenience-script). |
| Docker-Compose | https://docs.docker.com/compose/install/ | Follow the instructions for installation |
| nginx-proxy | https://hub.docker.com/r/jwilder/nginx-proxy, https://github.com/nginx-proxy/nginx-proxy | Docker image |
| nginx | https://hub.docker.com/_/nginx | Docker image |
| nginx letsencrypt companion | https://hub.docker.com/r/jrcs/letsencrypt-nginx-proxy-companion | Docker image |
| MariaDB | https://hub.docker.com/_/mariadb | MySQL Database Docker image for both NextCloud and Gitea |
| Gitea | https://hub.docker.com/r/gitea/gitea, https://gitea.io/en-us/ | Docker image |
| NextCloud | https://hub.docker.com/_/nextcloud | Docker image |