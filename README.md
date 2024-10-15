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

Follow these steps to configure Metadata Editor and run the docker container.

At the end of the process, your folder structure should look like this
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
|
└───app_data
    └───editor
    └───tmp
|...
│
```

To download the docker and docker source code and setup the project folder structure using CLI, run these commands:.

```bash
#clone repo 
git clone https://github.com/vbeinserm/metadata-editor-docker metadata-editor-docker

#switch to project folder
cd metadata-editor-docker

#download metadata editor source code into a subfolder
git clone https://github.com/ihsn/editor editor

#download metadata editor source code into a subfolder
git clone https://github.com/mah0001/pydatatools pydatatools

```

Before you can start the docker container, review the `docker-composer.yml` to make sure all settings are correct:

Apache port: The default port has been set to 8384, as nada docker uses 8383 by default

```yaml
    ports:
      - "0.0.0.0:8384:80"
```

## Data folder [app_data]

The Metadata Editor stores data in the `app_data` folder. Create the following folders in the project metadata-editor-docker root:

```bash
mkdir app_data
cd app_data
mkdir editor tmp
```


### Permissions in app_data

Set permissions for the `app_data` folder to allow the Apache server to read and write data.
Note : folder rights is dependent on your server's configuration.

```bash
 chgrp -R www-data  app_data
 chmod -R 775 app_data
```

## Set permissions for Metadata Editor folders
Set permissions for the `editor` folders to allow the Apache server to read and write data.
Note : folder rights is dependent on your server's configuration.

```bash
 cd editor

 chgrp -R www-data  files logs cache
 chmod -R 775 files logs cache
```

## Volume mapping

If you are using the project folder structure as above, there is no change needed. Otherwise, update the volume to map your local metadata editor folder.


```yaml
    volumes:
      - ./editor:/var/www/html/editor
      - ./app_data:/var/www/html/editor/datafiles
```


## Copy database.php to config folder

Editor does not come with a database.php file. You need to copy the database.php file from the docker folder to the `editor/application/config` folder.

```bash
cp database.php editor/application/config/database.php
```




## Start docker container

From the main folder where you have extracted the docker files, run this from command line:

```bash
docker compose up 

```

That should build the image and launch the containers. If everything works, you should be able to see the Metadata installer at http://localhost:8383/ (change 8383 to the port you used in docker compose)



### Setup
Depending on your installation, either use localhost (for local install) or server adresse (ip or url).
```
http://[localhost|server adress]:8384/
```
You should be redirected to install page.

Check the F**older READ/WRITE/DELETE permissions** section.
Everything should be ok, so adjust your folder permissions accordingly.

Then,
- Install Database
- Setup administrator

## TODO

- SMTP setup


    

- Credentials handling
  - In case of DDOS attack, using a require() on a .php file not in /var/www/html prevents the Database.php to show in browser directly
