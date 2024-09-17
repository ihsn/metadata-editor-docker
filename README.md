# Metadata Editor docker
[work in progress]



The docker image contains:

- Apache 2.4
- PHP 8.2
  - xsl
  - xml
  - mbstring
  - mysqli

- MySQL 5.x

Follow these steps to configure NADA and run the docker container.

1. Setup folders to host Metadata Editor Docker source and the Docker files

```
metadata_editor_docker 
|
| (clone the current repository)
|
| php.ini
| .docker/Dockerfile
| vhost.conf
| database.php
| ...
|
└───editor (metadata editor source code)
|
└───pydatatools 
|...
│
```

To download the docker and docker source code and setup the project folder structure using CLI, run these commands:.

```bash
#clone repo 
git clone https://github.com/vbeinserm/metadata-editor-docker metadata-editor-docke

#switch to project folder
cd metadata-editor-docker

#download metadata editor source code into a subfolder
git clone https://github.com/ihsn/editor

#download metadata editor source code into a subfolder
git clone https://github.com/mah0001/pydatatools

#copy database.php file to `application/config/database.php`
cp database.php nada/application/config/database.php
```

Before you can start the docker container, review the `docker-composer.yml` to make sure all settings are correct:

Apache port: The default is set to 8383, you may want to change that to port 80. *(N.B. needs to be 0.0.0.0 on my server)*

```yaml
    ports:
      - "0.0.0.0:8383:80"
```

Mount volume: If you are using the project folder structure as above, there is no change needed. Otherwise, update the volume to map your local metadata editor folder.

```yaml
    volumes:
      - ./editor:/var/www/html/editor
```



*Copy database.php to config folder*



Run the following commands to set read/write permissions for the folders where the data will be stored:

```yaml
 cd editor

 chgrp -R www-data  datafiles files logs
 chmod -R 775 datafiles files logs
```



## Start docker container

From the main folder where you have extracted the docker files, run this from command line:

```bash
docker compose up 

```

That should build the image and launch the containers. If everything works, you should be able to see the Metadata installer at http://localhost:8383/.



### Setup


### Fixed
- user rights with docker (might need to be adjusted to every host)


## TODO

- Mysql : upgrade to 8
- SMTP setup
- pydatatools setup



## Ideas

- Create a common docker for Nada & Metadata Editor ?

  - Easier to have a fully fuctionnal environment
  - No need to use different port for Nada & Metadata Editor
  - One mysql container with two databases

- Push the code inside the docker and not in volume?

  - Automatize git clone

    

- Credentials handling
  - In case of DDOS attack, using a require() on a .php file not in /var/www/html prevents the Database.php to show in browser directly
