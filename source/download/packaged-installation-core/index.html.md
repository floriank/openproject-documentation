# Packaged Installation Core

## OpenProject&nbsp;installation via package manager

The installation of the OpenProject software can be done manually or via official software-packages build by the packager.io service. Using these software packages is highly recommended to reduce the pain of installation and configuration errors. Besides the installation via package manager is done via configuration wizard which is very helpful to get everything up and running right from the beginning.

### Stack used by packager.io packages

- Unicorn (application server) – this component hosts the actual application. Depending on the settings chosen, there are at least two unicorn processes running in parallel on the app server machine.
- Apache 2 (web server) – this component provides the external interface, handles SSL termination (if SSL is used) and distributes/forwards web requests to the unicorn processes.
- MySQL (database management system) – this component is used to store and retrieve data.
- Ruby 2.1.3 (MRI) and necessary libraries to run the OpenProject source code.

## Run Installation

The installation procedure assumes the following prerequisites to be met:

- A server running one of the following Linux distributions to perform the installation on:
  - **Debian 7.6 Wheezy 64 bits server**
  - **Ubuntu 14.04 Trusty 64bits server**
  - **Fedora 20 64bits server**
  - **CentOS 6 / RHEL 6 64 bits server&nbsp;**

- A mail server that is accessible via SMTP that can be used for sending notification emails. OpenProject supports authentication, yet does not provide support for SMTP via SSL/TLS.

If you intend to use SSL for OpenProject:

- A valid SSL certifificate along with the private key file. The files must not be protected by a pass phrase.
- Note that as the installation procedure will automatically create a separate user to run OpenProject as, it is not necessary to create such a user beforehand.

The following steps have to be performed to initiate the actual installation of OpenProject via package manager. Note that all commands should either be run as root or should be prepended by sudo otherwise (typically depending what linux distribution is used).

### Debian&nbsp;7 Wheezy 64bits server

    # install https support
    apt-get install apt-transport-https
    
    
    sudo wget -qO - https://deb.packager.io/key | apt-key add -
    echo "deb https://deb.packager.io/gh/opf/openproject wheezy stable" | sudo tee /etc/apt/sources.list.d/openproject.list
    sudo apt-get update
    sudo apt-get install openproject

### &nbsp;Ubuntu 14.04 Trusty 64bits server

    wget -qO - https://deb.packager.io/key | sudo apt-key add -
    echo "deb https://deb.packager.io/gh/opf/openproject trusty stable" |
    sudo tee /etc/apt/sources.list.d/openproject.list
    sudo apt-get update
    sudo apt-get install openproject

### &nbsp;Fedora 20 64bits server

    sudo rpm --import https://rpm.packager.io/key
    echo "[openproject]
    name=Repository for opf/openproject application.
    baseurl=https://rpm.packager.io/gh/opf/openproject/fedora20/stable
    enabled=1" | sudo tee /etc/yum.repos.d/openproject.repo
    sudo yum install openproject

### CentOS / RHEL 64bits server

    sudo rpm --import https://rpm.packager.io/key
    echo "[openproject]
    name=Repository for opf/openproject application.
    baseurl=https://rpm.packager.io/gh/opf/openproject/centos6/stable
    enabled=1" | sudo tee /etc/yum.repos.d/openproject.repo
    sudo yum install openproject

## Post-Install Configuration

After the installation of the OpenProject package the system has to be configured to use this package and operate the OpenProject application. Therefore the package includes aconfiguration wizard which can be started using the following command.

    openproject configure

**Side note:** The installer supports the configuration of necessary SSL connections too. If required the corresponding SSL certificates (incl. keys) have to be placed on the machine.

## OpenProject command line tool

The `openproject` package comes with a command line tool to help manage important configuration settings. To see all possible command options of this tool type `sudo openproject`.

    admin@openproject-demo:~# sudo openproject
        Usage:
          openproject run COMMAND [options]
          openproject scale TYPE=NUM
          openproject logs [--tail|-n NUMBER]
          openproject config:get VAR
          openproject config:set VAR=VALUE
          openproject reconfigure

### Set configuration options

During the installation process a lot of settings were set to get the application runnings. But if you need to set some advanced options or you want to change some settings which were set during the installation via the installation wizard you can use `config:set` option. Please be aware that you have to stop and restart the application server so that the new configuration is loaded. This can be done using the `service openproject [start|stop]` command. See which settings can be set and an example for setting the session store below.

    DATABASE_URL=mysql2://openproject:9ScapYA1MN7JQrPR7Wkmp7y99K6mRHGU@127.0.0.1:3306/openproject
        SECRET_TOKEN=c5aa99a90f9650404a885cf5ec7c28f7fe1379550bb811cb0b39058f9407eaa216b9b2b22d27f58fb15ac21adb3bd16494ebe89e39ec225ef4627db048a12530

    ADMIN_EMAIL=mail@example.com
        EMAIL_DELIVERY_METHOD=smtp
        SMTP_DOMAIN=10.10.3.6
        SMTP_HOST=127.0.0.1
        SMTP_PASSWORD=mail
        SMTP_PORT=25
        SMTP_URL=smtp://mail:mail@127.0.0.1:25/10.10.3.6
        SMTP_USERNAME=mail

    WEB_CONCURRENCY=2
    WEB_TIMEOUT=15

    RAILS_CACHE_STORE=memcache
    SESSION_STORE=cach_store

### Example change session store

    sudo service openproject stop
    sudo openproject config:set SESSION_STORE="active_record_store"
    sudo service openproject start

**Attention** : Be aware that you should only set or change settings which are directly related to the openproject application/instance itself (see the list above). For example settings related to the apache web server can also be set via the `config:set` option but this can end up in an undesired state of the openproject application. If you want to change these settings use the `reconfigure` option. This will bring up the installation wizard again. See the “Reconfigure via wizard” section for more details on that.

## Run commands like rake tasks or rails console

The openproject command line tool supports running rake tasks and known scripts like the rails console. See example below how to do that.

    sudo openproject run rails console

or rake task

    sudo openproject run rake db:migrate

## Show logs

The command line tool can also be used to see the log information. The most typically use case is to show/follow all current log entries. This can be accomplished using the the –tail flag. See example below.

    sudo openproject logs --tail

## Reconfigure via wizard

As already mentioned in section “Set configuration options” if it is necessary to reset all settings defined during the installation process the reconfigure option can be used.

    sudo openproject reconfigure

The command above will bring up the installation wizard again. Please be aware that it will start the configuration/installation process from scratch. That is why it won’t show entries you already made. So you have to fill out every form again.

## Upgrade to a newer version

Upgrading the OpenProject is as easy as installing a newer OpenProject package and running the `openproject configure` command.

### Debian / Ubuntu

    sudo apt-get update
    sudo apt-get install --only-upgrade openproject
    sudo openproject configure

### Fedora / CentOS / RHEL

    sudo yum update
    sudo yum install openproject
    sudo openproject

