# Installation

--------------------------------------------------------
* [Requirements](#requirements)
* [Installation](#Installation)
	* [With Git](#install-with-git)
	* [With Composer](#install-with-composer)
	* [Deployment Ready](#deployment-ready)
* [Framework Only](#framework-only)
--------------------------------------------------------

### Requirements

* GIT
* PHP >= 7.2
* PHP Extensions:
	* OpenSSL PHP Extension
	* PDO PHP Extension
* Composer
* Web Server With SQL

--------------------------------------------------------

### Installation
Kanso can be installed one of two ways - [With Git](#install-with-git) or [With Composer](#install-with-composer). The recommended way to install Kanso is with Git as this gives you access to the the deployment service. This gives you access to deployment and update functionality to both Kanso and your application with Github webhooks.

--------------------------------------------------------

### Install With Git
Kanso can be installed and maintained via [Git](https://git-scm.com/). The great thing about this method is that you can customize your website or app however you like while still being able to update to the latest Kanso revision. This is the preferred way to run and maintain a Kanso application. To get started:

1. Open terminal and change the current working directory to the location where you want the cloned directory to be made.

```bash
cd <path/to/project>
```

2. Clone the Kanso repository.

```bash
git clone https://github.com/kanso-cms/cms.git .
```

3. You'll also need to create a database for Kanso on your web server, as well as a MySQL user who has all privileges for accessing and modifying it. You can create a database using phpMyAdmin or whatever means you prefer.

4. Add your database credentials into the `app/configurations/database.php` file. The default credentials are setup to run on most localhost default web servers.

5. You'll then need to rename `kanso/Install.sample.php` to `Install.php` so that the CMS installation can be instantiated.

6. Run the CMS installation script by accessing the installation URL in a web browser. This should be your webiste root e.g `http://example.com`.

7. You'll have to make the `app/storage` and `app/public/uploads` directories writable (command my vary depending on your system):

```bash
chown www-data:www-data -R app/storage
chown www-data:www-data -R app/public/uploads
```

8. Finally add a `.gitignore` file to your `app/configurations` directory to ensure any custom configuration settings you make are not overwritten.

```
*
!.gitignore
!defaults/*
```

#### Updating

Kanso can easily be updated when a new version is released using the following command:

```bash
git pull
```

--------------------------------------------------------
### Install With Composer

Installing Kanso is easy and can be with done in a few simple steps thanks to composer.

1. First you'll have to create a new project:

```bash
# cd /home/public_html
composer create-project kanso/cms .
```

2. Next you'll have to make the `app/storage` and  `app/public/uploads` directories writable (command my vary depending on your system):

```bash
chown www-data:www-data -R app/storage
chown www-data:www-data -R app/public/uploads
```

3. You'll also need to create a database for Kanso on your web server, as well as a MySQL user who has all privileges for accessing and modifying it. You can create a database using phpMyAdmin or whatever means you prefer.

4. Add your database credentials into the `app/configurations/database.php` file. The default credentials are setup to run on most localhost default web servers.

5. You'll then need to rename `kanso/Install.sample.php` to `Install.php` so that the CMS installation can be instantiated.

6. Run the CMS installation script by accessing the installation URL in a web browser. This should be your webiste root e.g `http://example.com`.

7. Finally you'll have to make the `app/storage` and  `app/public/uploads` directories writable (command my vary depending on your system):

```bash
chown www-data:www-data -R app/storage
chown www-data:www-data -R app/public/uploads
```

> Please see the [Configuration](/getting-started/configuratio) section on configuration your Kanso installation.

#### Updating

Kanso can easily be updated when a new version is released using the following command:

```bash
composer update
```

--------------------------------------------------------

### Deployment Ready
For production-ready environments, you may want to install Kanso using your own Github repository that you can push/pull changes to.

The advantages with this installation method is that you will have your own repository on Github for your project which you can deploy directly to your server using [Webhooks](https://developer.github.com/webhooks/).

1. On Github, [create a fork](https://help.github.com/articles/fork-a-repo/) of the `kanso-cms/cms` repository.

2. Clone your kanso fork to your local machine using [Github Desktop](https://desktop.github.com/).

3. Using terminal, navigate to your fork's directory.

4. Add a second remote to the original Kanso repository.

```bash
git remote add upstream https://github.com/kanso-cms/cms.git
```

#### Updating

Kanso can easily be updated when a new version is released using the following command:

```bash
git fetch upstream
git checkout master
git merge upstream/master
```

> For further details on deploying your project see the [Deployment](/deployment) section of the documentation.


--------------------------------------------------------

### Framework Only
Kanso can also be used as a framework only for more complex projects or projects that do not require a CMS. To use the framework only without the CMS:

1. Follow one of the installation methods above.

2. Open up the application configuration file found at `app/configurations/application.php`.

3. Remove all services under the `services/cms` key, so that it is just an empty array.

4. Kanso will not load the CMS and only the framework services will be available.

Find out more about [services](/getting-started/services) and [configurations](/getting-started/configuration)