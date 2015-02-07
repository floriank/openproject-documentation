# OpenProject 3.0 to OpenProject 4.0 Debian/Ubuntu Upgrade Guide

This guide describes the upgrade process from OpenProject 3.0 to 4.0 on Debian 7.7 and Ubuntu 14.04 LTS step by step.  
Note: We strongly recommend to update your OpenProject installation to the latest available 3.0 version (currently 3.0.14), before attempting an update to 4.0.

&nbsp;

## 1. Preparation

- Before Updating, check that all the installed OpenProject plugins support the new OpenProject Version. Remove incompatible plugins before attempting an upgrade.
- Stop the OpenProject Server. You may even add a maintenance page, if you feel comfortable with that (for example using the approach taken in [this blog post](http://blog.flowdock.com/2009/03/11/stopping-your-rails-application-with-phusion-passenger/))
- Please [do a backup](/download/backup-guide/ "Backup Guide")
- Stop the (delayed\_job) worker process.  
If you run the worker process with a cronjob, disable the cronjob temporarily.  
In case you run the woker process through `RAILS_ENV=production bundle exec script/delayed_job start`, execute the following:

    RAILS_ENV=production bundle exec script/delayed_job stop

Update your system:

    [root@debian]# apt-get update
    [root@debian]# apt-get upgrade

## 2. Get the new OpenProject Source Code

Change into the directory where OpenProject was installed as the operating-system-user OpenProject runs as. We assume that OpenProject is installed in `/home/openproject/openproject` by the `openproject` user.

    [root@debian]# su - openproject -c "bash -l"
    [openproject@debian]# cd ~/openproject/openproject

Remove manual changes and modifications (If you have modified OpenProject source files and want to preserve those changes, back up your changes, and re-apply them later):

    [openproject@debian]# git reset --hard
    [openproject@debian]# git checkout stable
    [openproject@debian]# git pull

## 3. Upgrade Ruby

OpenProject 4.0 requires Ruby to be installed in version 2.1.x. Assuming you have installed Ruby via [RVM](https://rvm.io/), do the following to upgrade your Ruby installation:

    [openproject@debian]# rvm get stable
    [openproject@debian]# export -f rvm_debug
    [openproject@debian]# rvm install 2.1.4
    [openproject@debian]# rvm use --default 2.1.4
    [openproject@debian]# gem install bundler
    [openproject@debian]# bundle install

## 4. Node.js installation

The major change from OpenProject 3.0 to 4.0 (when looking at the system requirements and installation) is how the assets (JavaScript and CSS) are precompiled. The precompilation step needs Node.js now.  
We will install the latest 0.10.x version of Node.js via [nodeenv](https://pypi.python.org/pypi/nodeenv) :

    [openproject@debian]# exit
    [root@debian]# apt-get install python python-pip
    [root@debian]# pip install nodeenv
    [root@debian]# su - openproject -c "bash -l"
    [openproject@debian]# cd /home/openproject
    [openproject@debian]# nodeenv nodeenv
    [openproject@debian]# source ./nodeenv/bin/activate
    [openproject@debian]# npm -g install bower

As a reference, the following Node.js and NPM versions have been installed on our system:

    [openproject@debian]# node --version
                          v0.10.32
    [openproject@debian]# npm --version
                          1.4.28
    [openproject@debian]# bower --version
                          1.3.12

## 5. The Upgrade

Now that the sources and dependencies are in place, you can migrate the Database and do the upgrade:

    [openproject@debian]# cd /home/openproject/openproject
    [openproject@debian]# npm install
    [openproject@debian]# bower install
    [openproject@debian]# RAILS_ENV="production" bundle exec rake db:migrate
    [openproject@debian]# RAILS_ENV="production" bundle exec rake db:seed
    [openproject@debian]# RAILS_ENV="production" bundle exec rake assets:precompile
    [openproject@debian]# touch tmp/restart.txt

## 6. Serving OpenProject

This sections only applies to you, if you serve OpenProject via Apache and Passenger. If you serve OpenProject in a different way, be sure to check that it still works.

During the 3.0 installation, we have set-up Passenger in the Apache configuration files. During the OpenProject upgrade, we have potentially installed a new Ruby and Passenger Version. The versions of Ruby and Passenger appear in the Apache configuration like this:

```apache
  LoadModule passenger_module /home/openproject/.rvm/gems/ruby-2.1.4/gems/passenger-4.0.53/buildout/apache2/mod_passenger.so
  <IfModule mod_passenger.c>
    PassengerRoot /home/openproject/.rvm/gems/ruby-2.1.4/gems/passenger-4.0.53
    PassengerDefaultRuby /home/openproject/.rvm/gems/ruby-2.1.4/wrappers/ruby
  </IfModule>
```

Please run the following commands to upgrade passenger and re-install the Apache module:

    [openproject@debian]# gem update passenger
    [openproject@debian]# gem cleanup passenger
    [openproject@debian]# passenger-install-apache2-module

The output of `passenger-install-apache2-module2` tells you how to configure Apache.  
It is basically the same as what is already installed, except for the updated version numbers.

Don’t forget to restart apache after the configuration change:

    [root@debian]# service apache2 reload

## 7. The Aftermath

- Re-enable the `delayed_job` cron job that was disabled in the first step.
- If you have put up a maintenance page, remove it.
- Start the OpenProject server again
- Watch for further OpenProject updates in our [news](https://community.openproject.org/projects/openproject/news), or on [twitter](https://twitter.com/openproject).

## 8. Questions, Comments, and Feedback

If you have any further questions, comments, feedback, or an idea to enhance this guide, please tell us at the appropriate [forum](https://community.openproject.org/projects/openproject/boards/9).

Also, please take a look at the [Frequently Asked Questions](/help/faq/upgrading-current-version-questions/ "Upgrading to the Current Version Questions").
