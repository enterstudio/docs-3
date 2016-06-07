---
title:  Server Requirements - Windows
category: Install Site Stacker
date: 2016-05-17 00:00:00
---

## 1. Recommended Server Hardware

> Note: The server hardware configuration varies widely between hosting providers and greatly depends on the amount of traffic expected. The following are server recommendations that will fit most light to moderate cases.

* Processor: 2.5 GHz (4 Cores)
* Memory: 8GB
* Storage: 80GB


## 2. Operating System

* Windows Server 2012 or newer


## 3. Access to the server

* Remote Desktop Access (username/password)
* Temporary Admin permissions are needed.
* Need to assign a temporary URL to the server for the duration of the project (sub domains are fine.)


## 4. Web Server

* Internet Information Services (IIS) 7.5 or newer
* Modules:
   * [URL Rewrite](http://www.google.com/url?q=http%3A%2F%2Fwww.iis.net%2Fdownloads%2Fmicrosoft%2Furl-rewrite&sa=D&sntz=1&usg=AFQjCNHslW_84Io3PnG3NyHM07RQ0Jzy8Q)
* IIS must have permission to:
   * Create symlinks
      * **The default IIS user “IUSER” cannot create symlinks. A different user must be created.**
   * Modify any file within the Site Stacker installation

### IIS Settings

`Activity Timeout` and `Request Timeout` [FastCGI Settings](https://www.iis.net/configreference/system.webserver/fastcgi) need to be increased to a large enough value (e.g. `99999`) because these are controlled from PHP. You can do this from the IIS Manager UI by following [these steps](http://stackoverflow.com/a/35756128/1104534).

## 5. Database Server

* Microsoft SQL Server 2012 or higher
  * The connection to the database server can be made with either SQL Authentication or Windows Authentication
     * If Windows Authentication will be used the user under which IIS process is running must map to a valid SQL Server login in order to establish a connection.
  * Optional, but highly recommended:
     * SQL Server Management Studio
  * Note: Use of MySQL with Windows Server is NOT an option.

### Database Access

The user used to connect to the database server needs the following access:

- Server Roles (server-wide security privileges): `bulkadmin`, `dbcreator`
- User Mapping (database role membership): `db_owner` (on the designated database)

If `dbcreator` cannot be granted, a second (empty) database called `sitestacker_test` needs to be created and the user should be a `db_owner` on that database.


## 6. PHP

* PHP 5.6.x or latest stable version
   * Download PHP [here](http://php.iis.net/)
   * PHP must also be available as a command in CLI
* Extensions:
   * curl
   * gd2
   * [iconv](http://php.net/manual/en/iconv.installation.php) (enabled by default, but can be disabled with `--without-iconv`)
   * mbstring (needed by CakePdf)
   * openssl (with TLS v1.2)
   * pdo_sqlsrv
   * soap
   * sockets
   * [wincache 1.3.5+](http://windows.php.net/downloads/pecl/releases/wincache/) (optional, recommended)
      * WinCache must be disabled in CLI (`wincache.enablecli=Off`) - default is Off
      * If WinCache 1.3.7 or newer is installed: `wincache.reroute_enabled=Off`
      * `wincache.ucenabled=On`
      * `wincache.ucachesize=85`
* Settings:
   * short_open_tag=On
   * allow_url_fopen=On
   * session.auto_start=Off
   * session.cookie_httponly=1
   * date.timezone=[`the server’s timezone`]
   * memory_limit=512M
   * soap.wsdl_cache_enabled=On
   * soap.wsdl_cache_dir=C:\Windows\Temp
   * DOMDocument (don’t use `--disable-dom` when compiling PHP)


## 7. Java

* Optional, if Search component is used:
   * We recommend installing the **Java 8 update 45 or later**, or **Java 7 update 79 or later**. Previous versions of Java 7 are known to have bugs that can cause index corruption and data loss. Only Oracle’s Java and the OpenJDK are supported. The same JVM version should be used on all Elasticsearch nodes and clients.


## 8. ElasticSearch

* Install from here [Elasticsearch](https://github.com/sitestacker/sitestacker-wiki/wiki/Install-elasticsearch)
* The official guide can be found [Here](https://www.google.com/url?q=https%3A%2F%2Fwww.elastic.co%2Fguide%2Fen%2Felasticsearch%2Freference%2Fcurrent%2Fsetup.html&sa=D&sntz=1&usg=AFQjCNG1Wa040IUIoTIfd3GoEbFqbH_o9Q)
* Is it very important that ElasticSearch be setup to run always as a service and will restart automatically after server reboots. To do this you can run `bin/service.bat install` and then in **Services** selected the `Restart the service` policy on failure.


## 9. Mail Server

* The server will need the default mail server setup and running.  Using the default mail server that comes with the OS should work with PHP without any tweeking.  (Changing the default mail server to something else has the potential of interrupting the interaction with PHP.) Make sure the mail server is setup and working.  


## 10. Backup

* It is very important to make sure daily backup is in place.  Because much of the project is labor and setup, it is critical this be in place and working.
* We recommend that a backup audit be done a couple times to insure all is well.
