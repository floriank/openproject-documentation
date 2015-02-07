# System Requirements

&nbsp;

## 
# Server

### Operating system

- Ubuntu `12.04 LTS`
- Ubuntu `14.04 LTS`
- Debian `7.2` (wheezy)
- Suse SLES 11.3
- CentOS /RHEL 6.4

### Gems

- Ruby `2.1.0`
- Rails `3.2.18`
- Other gems, as listed [here](https://github.com/opf/openproject/blob/stable/Gemfile)

### Databases

- MySQL `5.6.13`
  - `5.6.0`-`5.6.12` are not supported, because of a [MySQL Bug #69471](http://bugs.mysql.com/bug.php?id=69471)

- PostgreSQL `9.1.9`

### Webserver

- Passenger Gem version `4.0.19`
- apache webserver `2.2.22`

### Optional Server Components

- SCM binaries available in your PATH
  - svn version `1.7.5`
  - git version `1.8.4`

- ImageMagick (via the `RMagick` gem)

**Note**

Other configurations (operating systems, ruby versions, ..) might also work. But we test against the configuration listed above.

&nbsp;

## 
# Client

OpenProject supports the latest versions of the major browsers. In our strive to make OpenProject easy and fun to use we had to drop support for some older browsers.

### OpenProject 4.0

- &nbsp; **Mozilla Firefox (Version >= 31 ESR)**
  - If you are using the browser for personal use, you can rely on the [latest default version](http://www.mozilla.org/en-US/firefox/new/). If you are part of an organisation and updating your browser is not an option, please check out the [ESR](https://www.mozilla.org/en-US/firefox/organizations/faq/)

- **Google Chrome (current version)**
  - [Chrome’s current release](https://www.google.com/intl/en/chrome/browser/)

- **Microsoft Internet Explorer (Version >= 10)**
  - Although more than one version is supported, we recommend using the [current release](http://windows.microsoft.com/en-US/internet-explorer/download-ie)

### OpenProject 3.0

- **Mozilla Firefox (Version 24 – Version 32)**
  - If you are using the browser for personal use, you can rely on the [latest default version](http://www.mozilla.org/en-US/firefox/new/). If you are part of an organisation and updating your browser is not an option, please check out the [ESR](https://www.mozilla.org/en-US/firefox/organizations/faq/)

- **Google Chrome (Version 30 – Version 37)**
  - [Chrome’s current release](https://www.google.com/intl/en/chrome/browser/)

- **Microsoft Internet Explorer (Version 10 – Version 11)**
  - Although more than one version is supported, we recommend using the [current release](http://windows.microsoft.com/en-US/internet-explorer/download-ie)

&nbsp;

## 
# Supported screen reader (accessibility)

- JAWS 14.0 and higher
