# Manual Installation of OpenProject 4.0 with Apache on Debian 7.7 or Ubuntu 14.04 LTS

**This guide &nbsp;helps you to deploy OpenProject 4.0. Please, be aware that:**

1. This guide requires that you have a clean **Debian 7.7 x64** or **Ubuntu 14.04 x64** installation with administrative rights. We have tested the installation guide on a Debian minimal netinstall image and on an Ubuntu Server image, but it should work on any derivative.
2. OpenProject will be installed with a MySQL database (the guide should work analogous with PostgreSQL).
3. OpenProject will be served in a production environment with Apache (this guide should work analogous with other servers, like nginx and others)

In this guide, we will install **OpenProject 4.0** with a **MySQL** database. Openproject will be served with the **Apache** web server. When your server needs to reboot, OpenProject should start automatically with your server.

Note: We have highlighted commands to execute like this

    [user@host] command

Where the `user` is the operating system user the command is executed with. The `host` is either `debian` (when the command is Debian-specific), `ubuntu` (when the command is Ubuntu-specific), or `all` (when the command shall be executed on either operating system).

If you find any bugs or you have any recommendations for improving this tutorial, please, feel free to create a pull request against this guide.

## Prepare Your Environment

Install tools needed to compile Ruby and run OpenProject:

### Only on Debian

    [root@debian] apt-get update
    [root@debian] apt-get install git curl build-essential zlib1g-dev libyaml-dev libssl-dev libmysqlclient-dev libpq-dev libsqlite3-dev memcached libffi5

### &nbsp;Only on Ubuntu

    [root@ubuntu] apt-get update
    [root@ubuntu] apt-get install git curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libmysqlclient-dev libpq-dev libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties memcached libgdbm-dev libncurses5-dev automake libtool bison libffi-dev

### &nbsp;Debian and Ubuntu

Create a dedicated user for OpenProject:

    [root@all] groupadd openproject
    [root@all] useradd --create-home --gid openproject openproject
    [root@all] passwd openproject (enter desired password)

## Install Database (MySQL) Packages

During installation, you have to enter a password for the mysql root-user.

    [root@all] apt-get install mysql-server mysql-client

As a reference, we have installed the following MySQL version:

    [root@all] mysql --version
                  mysql Ver 14.14 Distrib 5.5.40, for debian-linux-gnu (x86_64) using readline 6.3

Create the OpenProject MySQL-user and database:

    [root@all] mysql -u root -p

You may replace the string `"openproject"` with the desired username and database-name. The password`"my_password"` should definitely be changed.

    mysql> CREATE DATABASE openproject CHARACTER SET utf8;
    mysql> CREATE USER 'openproject'@'localhost' IDENTIFIED BY 'my_password';
    mysql> GRANT ALL PRIVILEGES ON openproject.* TO 'openproject'@'localhost';
    mysql> \q

## Install Node.js

We will install the latest 0.10.x version of Node.js via [nodeenv](https://pypi.pythn.org/pypi/nodeenv):

    [root@all] apt-get install python python-pip
    [root@all] pip install nodeenv

## Install Ruby

Switch to the dedicated OpenProject-user (user `openproject` in our case):

    [root@all] su openproject -c "bash -l"

Switch to the user’s home directory …

    [openproject@all] cd ~

… and install RVM (Ruby Version Manager)

    [openproject@all] \curl -sSL https://get.rvm.io | bash -s stable

It can be that curl fails to download the RVM source, because of the missing GPG key. If that is the case, download the key (as suggested in the error message):

    [openproject@all] gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3

Then try to download RVM again and continue the installation with:

    [openproject@all] source $HOME/.rvm/scripts/rvm
    [openproject@all] rvm autolibs disable
    [openproject@all] rvm install 2.1.4
    [openproject@all] rvm use --default 2.1.4
    [openproject@all] gem install bundler

As a reference, we have installed the following version of bundler:

    [openproject@all] bundle --version
                      Bundler version 1.7.4

## Activate Node.js

    [openproject@all] cd ~
    [openproject@all] nodeenv nodeenv
    [openproject@all] source ./nodeenv/bin/activate
    [openproject@all] npm -g install bower

As a reference, the following Node.js and NPM versions have been installed on our system:

    [openproject@all] node --version
                      v0.10.33
    [openproject@all] npm --version
                      1.4.28
    [openproject@all] bower --version
                      1.3.12

## Install OpenProject

    [openproject@all] cd ~
    [openproject@all] git clone https://github.com/opf/openproject.git
    [openproject@all] cd openproject
    [openproject@all] git checkout stable
    [openproject@all] bundle install
    [openproject@all] npm install
    [openproject@all] bower install

## Configure OpenProject

Create and configure the database configuration file in `config/database.yml` (relative to the openproject-directory).

    [openproject@all] cp config/database.yml.example config/database.yml

Now edit the `config/database.yml` file and insert your database credentials. It should look like this (just with your database name, username, and password):

```yaml
  production:
    adapter: mysql2
    database: openproject
    host: localhost
    username: openproject
    password: openproject
    encoding: utf8

  development:
    adapter: mysql2
    database: openproject
    host: localhost
    username: openproject
    password: openproject
    encoding: utf8
```

Configure email notifications (using a gmail account as an example) by creating configuration.yml in `config` directory.

    [openproject@all] cp config/configuration.yml.example config/configuration.yml

Now, edit the `configuration.yml` file as you like.

```yaml
  production: #main level
    email_delivery_method: :smtp #settings for the production environment
    smtp_address: smtp.gmail.com
    smtp_port: 587
    smtp_domain: smtp.gmail.com
    smtp_user_name: ***@gmail.com
    smtp_password: ****
    smtp_enable_starttls_auto: true
    smtp_authentication: plain
```

Add this line into `configuration.yml` file at the of of file for better performance of OpenProject:

```yaml
  rails_cache_store: :memcache
```

**NOTE:** You should validate your .yml-files, for example with [http://www.yamllint.com/](http://www.yamllint.com/). Both, the `database.yml` and`configuration.yml` file are sensitive to whitespace. It is pretty easy to write invalid .yml files without seeing the error. Validating those files prevents you from such errors.

## Finish the Installation of OpenProject

    [openproject@all] cd ~/openproject
    [openproject@all] bundle exec rake db:create:all
    [openproject@all] bundle exec rake generate_secret_token
    [openproject@all] RAILS_ENV="production" bundle exec rake db:migrate
    [openproject@all] RAILS_ENV="production" bundle exec rake db:seed
    [openproject@all] RAILS_ENV="production" bundle exec rake assets:precompile

## Serve OpenProject with Apache and Passenger

OpenProject will be served by the Rails application server “Passenger”, and the apache webserver. We set up the system in a way, that automatically starts OpenProject with the operating system.

### Only on Debian

First, exit the current bash session with the `openproject` user, so that we are again in a root shell. Then, we prepare apache and passenger:

    [openproject@debian] exit
    [root@debian] apt-get install apache2 libcurl4-gnutls-dev apache2-threaded-dev libapr1-dev libaprutil1-dev
    [root@debian] chmod o+x "/home/openproject"

Now, the Passenger gem is installed and integrated into apache.

    [root@debian] su - openproject -c "bash -l"
    [openproject@debian] cd ~/openproject
    [openproject@debian] gem install passenger
    [openproject@debian] passenger-install-apache2-module

Follow the instructions passenger provides. The passenger installer will ask you the question in “Which languages are you interested in?”. We are interested only in ruby.

As told by the installer, add this lines to `/etc/apache2/apache2.conf`. But before copy&pasting the following lines, check if the content (especially the version numbers!) is the same as the `passenger-install-apache2-module` installer said. When you’re in doubt, do what passenger tells you.

```apache
  LoadModule passenger_module /home/openproject/.rvm/gems/ruby-2.1.4/gems/passenger-4.0.53/buildout/apache2/mod_passenger.so
  <IfModule mod_passenger.c>
    PassengerRoot /home/openproject/.rvm/gems/ruby-2.1.4/gems/passenger-4.0.53
    PassengerDefaultRuby /home/openproject/.rvm/gems/ruby-2.1.4/wrappers/ruby
  </IfModule>
```

As the root user, create the file `/etc/apache2/conf.d/openproject.conf` with the following contents:

```apache
  <VirtualHost *:80>
     ServerName www.myopenprojectsite.com
     # !!! Be sure to point DocumentRoot to 'public'!
     DocumentRoot /home/openproject/openproject/public
     <Directory /home/openproject/openproject/public>
        # This relaxes Apache security settings.
        AllowOverride all
        # MultiViews must be turned off.
        Options -MultiViews
        # Uncomment this if you're on Apache >= 2.4:
        #Require all granted
     </Directory>
  </VirtualHost>
```

### Only on Ubuntu

First, exit the current bash session with the `openproject` user, so that we are again in a root shell. Then, we prepare apache and passenger:

    [openproject@ubuntu] exit
    [root@ubuntu] apt-get install apache2 libcurl4-gnutls-dev apache2-threaded-dev libapr1-dev libaprutil1-dev
    [root@ubuntu] chmod o+x "/home/openproject"

As a reference, the following version of apache was installed:

    [root@ubuntu] apache --version

Now, the Passenger gem is installed and integrated into apache.

    [root@ubuntu] su - openproject -c "bash -l"
    [openproject@ubuntu] cd ~/openproject
    [openproject@ubuntu] gem install passenger
    [openproject@ubuntu] passenger-install-apache2-module

Follow the instructions passenger provides. The passenger installer will ask you the question in “Which languages are you interested in?”. We are interested only in ruby.

The passenger installer tells us to edit the apache config files. To do this, continue as the root user:

    [openproject@ubuntu] exit

As told by the installer, create the file `/etc/apache2/mods-available/passenger.load` and add the following line. But before copy&pasting the following lines, check if the content (especially the version numbers!) is the same as the `passenger-install-apache2-module` installer said. When you’re in doubt, do what passenger tells you.

```apache
  LoadModule passenger_module /home/openproject/.rvm/gems/ruby-2.1.4/gems/passenger-4.0.53/buildout/apache2/mod_passenger.so
```

Then create the file `/etc/apache2/mods-available/passenger.conf` with the following contents (again, take care of the version numbers!):

```apache
  <IfModule mod_passenger.c>
    PassengerRoot /home/openproject/.rvm/gems/ruby-2.1.4/gems/passenger-4.0.53
    PassengerDefaultRuby /home/openproject/.rvm/gems/ruby-2.1.4/wrappers/ruby
  </IfModule>
```

&nbsp;

Then run:

    [root@openproject] a2enmod passenger

As the root user, create the file `/etc/apache2/sites-available/openproject.conf` with the following contents:

```apache
  <VirtualHost *:80>
     ServerName www.myopenprojectsite.com
     # !!! Be sure to point DocumentRoot to 'public'!
     DocumentRoot /home/openproject/openproject/public
     <Directory /home/openproject/openproject/public>
        # This relaxes Apache security settings.
        AllowOverride all
        # MultiViews must be turned off.
        Options -MultiViews
        # Uncomment this if you're on Apache >= 2.4:
        Require all granted
     </Directory>
  </VirtualHost>
```

Let’s enable our new `openproject` site (and disable the default site, if necessary)

    [root@ubuntu] a2dissite 000-default
    [root@ubuntu] a2ensite openproject

### Debian and Ubuntu

Now, we (re-)start Apache:

    [root@all] service apache2 reload

Your OpenProject installation should be accessible on port 80 (http). A default admin-account is created for you having the following credentials:

    Username: admin
    Password: admin

Please, change the password on the first login. Also, we highly recommend to configure the SSL module in Apache for https communication.

## Activate Background Jobs

OpenProject sends (some) mails asynchronously by using background jobs. All such jobs are collected in a queue, so that a separate process can work on them. This means that we have to start the background worker. To automate this, we put the background worker into a cronjob.

    [root@all] su - openproject -c "bash -l"
    [openproject@all] crontab -e

Now, the crontab file opens in the standard editor. Add the following entry to the file:

    */1 * * * * cd /home/openproject/openproject; /home/openproject/.rvm/gems/ruby-2.1.4/wrappers/rake jobs:workoff 

This will start the worker job every minute.

## Follow-Ups

Your OpenProject installation is ready to run. However, there are some things to consider:

- Regularly backup your OpenProject installation. See the [backup guide](https://community.openproject.org/projects/openproject/wiki/Create_Backups) for details.
- Serve OpenProject via https
- Enable Repositories for your OpenProject projects
- Watch for OpenProject updates. We advise to always run the latest stable version of OpenProject (especially for security updates). You can find out about new OpenProject releases in our [news](https://community.openproject.org/projects/openproject/news), or on [twitter](https://twitter.com/openproject).

## Plug-In Installation (Optional)

This step is optional.

OpenProject can be extended by various plug-ins, which extend OpenProject’s capabilities. For general information and a list of all plug-ins known to us, refer to to the [plug-in page](/download/install-plugins/ "Install Plugins").

## Troubleshooting

You can find the error logs for apache here:

    /var/log/apache2/error.log

The OpenProject logfile can be found here:

    /home/openproject/openproject/log/production.log

If an error occurs, it should be logged there.

If you need to restart the server (for example after a configuration change), do

    [openproject@all] touch ~/openproject/tmp/restart.txt

## Questions, Comments, and Feedback

If you have any further questions, comments, feedback, or an idea to enhance this guide, please take a look at the [Frequently Asked Questions](/help/faq/manual-installation-questions/ "Manual Installation Questions")or tell us at the appropriate community [forum](https://community.openproject.org/projects/openproject/boards/9).&nbsp; [Follow OpenProject on twitter](https://twitter.com/openproject), and follow the news on [openproject.org](http://www.openproject.org/) to stay up to date.
