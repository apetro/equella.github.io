[Home](https://equella.github.io/)

# Open Source EQUELLA® Installation and Administration Guide

[Installation Prerequisities](#installation-prerequisites)

[Install a Database](#install-a-database)

[Install the Oracle Java SDK](#install-the-oracle-java-sdk)

[Install ImageMagick](#install-imagemagick)

[Install Libav](#install-libav)

[Install EQUELLA](#install-equella)

[Stop the EQUELLA server](#stop-the-equella-server)

[EQUELLA Server Adminstration Account](#equella-server-administration-account)

[Import a New Institution ](#import-a-new-institution)

[Manage Databases](#manage-databases)

[Use a reverse proxy server](#use-a-reverse-proxy-server)

[Customize the EQUELLA Digital Repository](#customize-the-equella-digital-repository)

[Use Log Files](#use-log-files)

[Thread Dump](#thread-dump)

[Health Check](#health-check)

[Uninstall EQUELLA](#uninstall-equella)



EQUELLA has a step-by-step wizard that guides administrators through the installation process. To run the installation wizard successfully, some preparatory work is required. EQUELLA requires a server license, a graphical user interface, ImageMagick™ for images, Libav for video files, a database for storing information and Oracle Java JDK™ to provide the low level structure on which EQUELLA runs.

## Installation prerequisites
EQUELLA installation involves the following steps:
1. Ensure a graphical user interface (GUI) is available (Windows, X-windows or equivalent). 
2. Ensure a suitable database or databases are available for EQUELLA. EQUELLA is supported on the following database servers:
* Oracle™ 10g, 11g and 12c 
* Microsoft SQL Server™  2008, 2008 R2 or 2012 and 2014
* PostgreSQL™ 8 or higher.
3. Ensure that Oracle Java JDK 8 is installed on the server.
4. Ensure ImageMagick is installed on the server.
5. If using video file attachments (other than streaming media) and require thumbnails and previews for those files, ensure Libav is installed on the server.
6. Install EQUELLA. 
7. Ensure the installation is functioning correctly.


## Install a Database

All databases must be able to store character data using UTF-8 encoding.

A database must be configured for use by EQUELLA with EQUELLA being the owner or having complete control of the database.

NOTE: When using multiple databases, only databases from one vendor may be used. For example, two Microsoft SQL Server databases could be used, but NOT a Microsoft SQL Server and a PostgreSQL Server database. The database vendor is selected when the EQUELLA system is first installed, and the database configured during the EQUELLA installation wizard is the default system database.
### Create an Oracle database instance for EQUELLA
Creating a database instance on Oracle should be managed by an experienced Oracle DBA. No step-by-step guide is provided, however to successfully install EQUELLA on an Oracle database (10g, 11g and 12c) the EQUELLA user (in the default install this is ‘equellauser’) must have the following:
* Permissions: to create, modify and delete tables, indexes and constraints and to run, select, insert, delete and update queries. 
* Database Name: the name must not exceed 20 characters.
* Database Username: the name must not exceed 20 characters.
* Character Encoding: the character encoding must be set to Use Unicode (AL32UTF8).

EQUELLA will use the default schema for this user, which will be the same as the username unless configured. This can be changed by creating an AFTER LOGON trigger to run ALTER SESSION SET CURRENT_SCHEMA = myschema

EQUELLA uses the default Users Tablespace that must have an <unlimited> Quota Size and the OPEN_CURSORS parameter should be set to 1000.

### Create a Microsoft SQL Server database instance for EQUELLA

If you are using Microsoft SQL Server 2008, 2012 or 2014, please ensure that the TCP/IP protocol has been enabled. The EQUELLA user (the installer default value is ‘equellauser’) must have the following:
* Permissions: to create, modify and delete tables, indexes and constraints and to run, select, insert, delete and update queries. 
* Database Name: the installer default name is ‘equella’.
* Database Role: the database user must be the database owner. For the EQUELLA user login select ‘db_owner’.

It is required that Microsoft SQL Server databases have READ_COMMITTED_SNAPSHOT enabled to avoid possible deadlocks. See http://msdn.microsoft.com/en-us/library/ms173763.aspx for more information. The following statement will enable this setting for a given database:

ALTER DATABASE MyEquellaDatabase SET READ_COMMITTED_SNAPSHOT ON;

### Create a PostgreSQL database instance for EQUELLA

Installations using a PostgreSQL (8.0 or higher) database must create an EQUELLA database before installing EQUELLA. The EQUELLA user (in the default install this is ‘equellauser’) must have the following:
* Permissions: to create, modify and delete tables, indexes and constraints and to run, select, insert, delete and update queries. 
* Database Name: the default name used is ‘equella’.
* Database Owner: the default name used is ‘equellauser’.
* Tablespace:‘pg_default’.

The database can now be used for an installation of EQUELLA.


## Install the Oracle Java SDK

The JDK can be obtained from Oracle at <http://www.oracle.com/technetwork/java/javase/downloads/index.html>. Installation instructions are available for all platforms by following the appropriate Oracle documentation. We recommend installing the latest point release of Java 8 JDK.

During installation the name and location of the folder in which the JDK is installed is needed to properly configure and run EQUELLA. 

The next step in the installation is to install ImageMagick.

## Install ImageMagick

ImageMagick can be obtained from <http://www.imagemagick.org/> . Download the platform-specific installer. EQUELLA requires version 6.4 or greater to be installed. 

NOTE: EQUELLA requires Imagemagick version 6.8.9 or higher for digital camera RAW image files. Additionally, a third party plugin called Ghostscript is required by Imagemagick to enable the generation of thumbnails for some file types (for example, pdfs). Go to <http://ghostscript.com> to download and install. 

For a full list of image file types supported by Imagemagick, go to <http://imagemagick.sourceforge.net/http/www/formats.html>.

Install the program, taking note of the name and location of the folder in which ImageMagick is installed as the EQUELLA installation will require these details to properly configure and run EQUELLA. 

## Install Libav

Install the program, taking note of the name and location of the folder in which the avconv and avprobe executable files have been installed, as the EQUELLA installation will require these details to properly configure and run EQUELLA.

### To install Libav for Windows
1. Go to <http://builds.libav.org/windows/release-gpl/> and download the relevant release build. 
2. Unzip to a directory of choice (e.g. Program Files) taking note of the name and location of the folder in which the avconv.exe and avprobe.exe files have been installed, as the EQUELLA installation will require these details to properly configure and run EQUELLA.
### To install and configure Libav for Linux
1. Install Libav from <https://libav.org/download.html>
libvo_aacenc and libx264 dependencies are also required for video previews to be generated correctly. Take note of the name and location of the folder in which the avconv and avprobe executables have been installed, as the EQUELLA installation will require these details to properly configure and run EQUELLA.


## Install EQUELLA

When installing EQUELLA, a wizard is provided that gathers information for the installation and initiates the installation process once sufficient information has been provided. If the installation fails to complete due to inappropriate initialization of the wizard, all partially installed components are removed and the wizard closes leaving the system as it was, prior to the installation attempt. 

### Before you install: important network configuration
EQUELLA has an inbuilt web server that stores content, and as such requires a port number to listen on. Additionally, EQUELLA needs to know the full URL by which linked content will be accessed.

When installing EQUELLA on a server running another web server, there are two options:
1. Assign a second IP Address to the Server for EQUELLA (recommended)—Assuming your machine already has one assigned IP address, such as 10.0.0.1, assign a second IP address to the same machine, for example 10.0.0.2, then create a DNS entry for this second IP address, such as EQUELLA.myinstitute.edu.au. This enables both EQUELLA and any other web server to run on port 80 and coexist on the same server.
2. Install EQUELLA to another server port (not recommended)—EQUELLA can be installed on another port, however this is not recommended as many firewall/proxy configurations will prevent access to this port.

For either option, you will need to make sure that your firewall/proxy will let all users of the system have access to the chosen port on the assigned IP address.

Since material created in EQUELLA is linked to content stored on the EQUELLA server, students as well as educators will need indirect access to this server. Usually users will be unaware of this access.

### Installation procedures

EQUELLA provides files for installation that can be used on all platforms. The installation process gathers the following information and settings:
* Directory in which to install EQUELLA
* Database access information
* Web server information
* EQUELLA administration settings

This information is required to successfully install EQUELLA. Incomplete details will cause the installation to fail.

### PLACEHOLDER FOR INSTRUCTIONS ON CREATING/OBTAINING INSTALLATION FILES FOR OPEN EQUELLA VIA APEREO

1.  ... In Progress...  Need to insert steps required to get install files 

### Begin EQUELLA Installation 

This procedure describes installing EQUELLA using a graphical interface. The examples shown are using Windows although other GUIs such as X-windows will be similar. The wizard pages provide information on the required details. Read each page before entering information.

1. PLACEHOLDER FOR WHERE THE INDIVIDUAL CAN DOWNLOAD THE equella-6.4-GAx Installer file.
2. Extract the equella-installer-xxxx.zip file to a temporary directory.
3. Navigate to the installer temporary directory and double-click on the enterprise-install file to start the installation. 
4. Click Next to display the Java Development Kit page.
5. Click Browse and navigate to the directory in which the JDK is installed.
6. Click Next to display the Install Directory page.
7. Click Browser to select the location where EQUELLA will be installed (e.g. ‘c:\equella’).
8. Click Next to display the Database Server page.  The database server group and database type must be entered to configure EQUELLA with an empty database ready for use. 
9. Select the database type from the drop-down list (e.g. ‘SQL Server’). 
10. Select a Database Server from the following options:
* This machine — select this option if the database server is on the local machine.
* A different server — select this option if the database server is not on this machine and enter the IP address or hostname of the server. 
11. Click Next to display the Database Authentication page. 
12. Enter the EQUELLA Database Name (the default value is ‘equella’).
13. Enter the Database Username (the default value is ‘equellauser’). 
(NOTE: For Oracle the database username must start with an alphabetic character.)
14. Enter the Database Password used to access the database. This is the password used to set up the database. A confirmation dialog is displayed.
15. Re-enter the password to confirm the password is correct. 
16. Click Next to display the Web Server Settings page.  The web server settings are used to allow web access to EQUELLA.
17. Enter the Institution Administration URL — enter the DNS name or IP address and port 
18. Select an Address Binding from the following options:
* Bind to all network interfaces — select this option if EQUELLA is the only web server running on the machine.
* Restrict to given hostname or IP address — select this option if EQUELLA is sharing the machine with other web servers.
19. Click Next to display the EQUELLA Manager page. The EQUELLA Manager is a separate service with its own authentication and port number.
20. Enter the password for the EQUELLA Manager website — choose a password to secure the EQUELLA Manager and note the password for future upgrades.
21. Enter the EQUELLA Manager website port number — 3000 is the default value. If this port is currently used set the port value to any unused port number.
22. Click Next to display the Proxy Server Settings page
23. Specify whether the EQUELLA server uses a proxy server to provide external access. Select from the following options:
* Direct Connection — select if no proxy server is used then select the Next button to skip the Proxy Server Configuration page and display the Memory Management page
* Proxy Server — select then select the Next button to display the Proxy Server Settings page
24. Proxy Server Settings
* Enter the Proxy Host and Proxy Port.
* Enter the Proxy Username and Proxy Password for proxy authentication. Leave blank if there is none.
* Click Next to display the Memory Management page. 
24. Memory Management page - The memory usage of EQUELLA can be set on this page. It is recommended that the default settings Minimum Memory Usage: 96m (MB) and Maximum Memory Usage: 512m (MB) are used. These provide suitable settings for machines with 1024 MB of memory where EQUELLA is the only application running. The Minimum Memory Usage should never be less than 96 MB nor should the Maximum Memory exceed the amount of physical RAM available on the server (the RAM used for the OS should be taken into consideration as well). Memory should be allocated to allow sufficient memory for all applications being run on the server.
(NOTE: For 32-bit systems, Java processes on Windows are limited to 1536 MB.)
25. Click Next to display the ImageMagick page.
26. Click Browse and navigate to the directory that contains the ImageMagick files. 
27. Click Next to display the Libav page
28. If you have installed Libav to create thumbnails and previews for video files uploaded as attachments, click Browse and navigate to the directory that contains the Libav files avconv.exe and avprobe.exe. Otherwise leave the field blank. A confirmation dialog displays to confirm that Libav is not required. 
29. Click Next display the Ready to Install page
30. Click Install to begin the installation process. The Installing… progress dialog may take a few moments to appear.
31. When completed select OK to confirm.
32. EQUELLA is now installed and the server is ready to be registered and started.

### Register the EQUELLA Server

#### To Register the EQUELLA server as a Windows service
1. Navigate to the Manager folder (the default installation folder is C drive :\equella\manager). In the ‘config’ files, details for the services can be edited (this is optional.) The details that can be changed include: 
* logging properties
* service names and descriptions 
* whether the service should auto start.
2. Optional: To change the details, navigate to the manager folder (<path-to-equella>\manager\) and edit the manager-config and/or equellaserver-config files as required.
3.  To register the EQUELLA server with Windows: Open a command prompt in the Manager folder (Shift/right click, then Open command window here) and enter:
```
manager install
equellaserver install
```
4. To start the service without restarting the machine either navigate to the manager folder (the default installation folder is C:\equella\manager), open a command prompt (Shift/right click, then Open command window here) and enter:
```
manager start
equellaserver start
```
Or
In Windows™ on the EQUELLA server, go to the Control Panel, Administrative Tools then double click Services.
Find the EQUELLA services (by default the names are EQUELLA App Server and EQUELLA Manager) in the list of services, right click and select the Start. 

The EQUELLA server is now started but may take a few minutes to be operational.
Once the server has been registered and started, the success of the installation can be checked by opening the Server administration account. 

#### Linux Installations
1. To start the service, navigate to the EQUELLA install directory, then the manager folder (e.g. path-to-equella/manager). From this folder, the server can be started by running the commands:
```
./manager start
./equellaserver start
```
Once the server has been started, the success of the installation can be checked by opening the Server administration account. 

## Stop the EQUELLA server


### To stop the EQUELLA server using Windows
1. Go to the Start menu, Control Panel, Administrative Tools then double click Services. 
2. Find EQUELLA in the list of services (by default, the names are EQUELLA App Server and EQUELLA  Manager), right click and select Stop. 

### To stop the EQUELLA server on other platforms
1. Navigate to the Manager folder (the default installation folder is /usr/local/equella), open a command prompt and enter:
```
./manager stop
./equellaserver stop
```
2. The services have now stopped. 


## EQUELLA Server Administration Account

The EQUELLA Server administration account is hidden from casual users and is displayed by entering a special URL created from the server’s base URL. 

### To open the Server administration account page
1. Open a browser and enter the EQUELLA address of the hosting server with 
‘/institutions.do?method=admin’ appended to the URL. (e.g. ‘http://equella.myinstitution.edu/logon.do’ would become ‘http://equella.myinstitute.edu/institutions.do?method=admin’).
2. The Server administration - Welcome page displays
2. Enter Email addresses, SMTP, No reply mail, User, SMTP password, and Confirm SMTP password.
3. Enter a System password, then Confirm password. This password is used to access the Server administration function in the future.
4. Click Install. The Databases page displays, with the system database listed.
5. Click the Initialize link to start the database initialization process. The progress percentage displays on the page.  During the initialization process, the button changes to Progress. Click this button to view the progress dialog.
6.  When the initialization process is complete, the database status changes to ‘Online’. 

## Import a New Institution
1. Select Import institution from the navigation menu to display the Import new institution page
2. Click Browse to select the institution zip file to import (e.g. institution-....tgz). 
3. Click to start the import. The Import new institution page displays. 
The Import New Institution page allows for arbitrary base URLs and the renaming of the institution.
4. To continue the importation, if multiple databases have been configured, click Select Database and select the required database in the Target database field. Otherwise the system defaults to the database set up during installation.
5. Enter an Institution name for the institution. The institution name must be unique for the EQUELLA server.
6. Enter an Institution URL for the institution. 
Server administrators are able to give institutions an arbitrary base URL. This URL may contain a base URL context. For example, the following base URLs would be valid for institutions on the same server: 
  * http://some.host.com/ 
  * http://another.host.com/ 
  * http://another.host.com/with/a/context/ 
  * http://another.host.com/with/another/context/ 
  * http://on.a.different.port:8080/ 

The arbitrary base URL can be entered in the Institution URL edit box. The Institution URL should be fully qualified. It is not possible to overwrite the other institution’s URL space, for example: ‘http://equella.myinstitution:4012/doco/qa2/’ will conflict with  ‘http://equella.myinstitution:4012/doco/’. This will be disallowed and will result in the following message:
**‘URL must not 'overwrite' an existing institution's URL space, in this case http://equella.myinstitution:4012/doco/qa2/. This may cause this institution to work incorrectly’.**

7. Enter a unique Filestore folder name. This is optional; if a name is not entered a folder with a randomly generated name will be automatically generated for the institution in the path-to-equella\filestore\Institutions folder.
8. Enter a new Admin password for the institution administrator. If left blank, the institution will inherit the password from the imported institution. (NOTE: This password is used to log in to the Institution using the TLE_ADMINISTRATOR login.)
9. Confirm the password.
10. Click Import new institution then click OK to confirm. An Importing… progress dialog that indicates the import progress is displayed. When the import is complete the Return of Institution Management button becomes active.
11. Click Return to Institution Management to view the new institution on the Institutions page.

Installation of the EQUELLA server is now complete. Login to the institution as the TLE_ADMINISTRATOR to administer and configure the institution.

## Manage Databases

EQUELLA provides the ability to use multiple databases, allowing each institution to have its own database. This improves both security and performance.

### Add a new database to EQUELLA procedure
Configuring multiple databases involves the following steps:
1. Install a new database. 
NOTE: When using multiple databases, only databases from one vendor may be used. For example, two Microsoft SQL Server databases could be used, but NOT a Microsoft SQL Server and a PostgreSQL Server database. The database vendor is selected when the EQUELLA system is first installed.
2. Configure the new database in EQUELLA. (See Add a database to EQUELLA below.)
3. Clone or import an institution, specifying the new database. 

### Add a database to EQUELLA
Databases are managed through the Databases function, accessed from the Server Administration page.

To add a new database:
1. Select Databases from the Server Administration page navigation menu. 
2. Click the Add database link to display the Add database dialog.
3. The Use system schema checkbox is only selected to default to the database specified in the hibernate.properties file. Selecting this checkbox disables the remaining fields.
4. Enter the new database JDBC URL. In this example, the existing EQUELLA database is PostgresSQL, so the system prompts a PostgreSQL example. If using MS SQL or Oracle, a relevant example is shown.
The existing database JDBC URL can be found in path-to-equella\learningedge-config\hibernate.properties, and the structure is 
```
jdbc:<databasetype>://<host>:<port>/<database_name>.
```
For example, jdbc:postgresql://localhost:5432/equella51. 

Change the <database_name> to the new database name. For example, jdbc:postgresql://localhost:5432/Equella2

5. Enter the database Username and Password. 
6. Click Add and bring online to save the Main connection details. The Database page displays, and the new database starts an automatic initialization process with a progress bar. Once completed, the new database displays with a status of Online.

### Reporting connection

The reporting connection section allows a separate database login for users who have reporting/read-only privileges.

To configure Reporting connection settings:
1. Enter the JDBC URL, if relevant. 
NOTE: This would only be different from the Main connection JDBC URL if data was transferred to a separate database for reporting purposes. Leave blank to use the Main connection JDBC URL.
2. Enter the Username and Password.
3. Click Save to save the details.

### Take a database offline

Databases can be taken offline if required. For example, for database maintenance or to take down a group of institutions.
1. From the Databases page, click Take Offline. A warning/confirmation dialog displays.
2. Click OK  to confirm. The database Status is now Offline

### Bring a database online

If a database has been taken off-line, it must then be brought back online for use.
1. From the Databases page, click Bring Online. The database Status is now Online. 

### Edit database settings
Database settings can be accessed for editing. Generally settings won’t be changed, but a Reporting connection or description might be added at a later date. The database must be Offline for the settings to be edited. 
1. From the Databases page, click the drop-down in the relevant database’s Actions column to view menu options.
2. Select the Edit database settings link to display the Edit database page.
3. Make the relevant changes, then click Save.

### Delete a database
On rare occasions, it may be required to delete a database that may no longer be used. The database must be Offline before it can be deleted. 
1. From the Databases page, click the drop-down in the relevant database’s Actions column to view menu options.
2. Select the Remove this database link. A warning/confirmation dialog displays.
3. Click OK to confirm.

### Reload database state
The Reload database state function checks the database state and reloads if required. This function might be used if changes had been made to a database, or if a database had become unavailable to EQUELLA due to technical issues, but is again available.
1. From the Databases page, click the drop-down in the relevant database’s Actions column to view menu options.
2. Select the Reload database state link.


## Use a Reverse Proxy Server

This section describes how to configure EQUELLA to run with a reverse proxy.

A reverse proxy is a gateway for servers enabling one web server to provide transparent access to content from multiple servers and also provides controlled access from the Internet. Possible reasons for running EQUELLA from a reverse proxy include:
* providing all services through a single server
* providing an EQUELLA service on the default HTTP port (or any below 1024) but running the service as a non-privileged user
* increasing the security of your services by running Apache or Squid
* you want to use SSL with EQUELLA 

The following section provides an example installation using Apache. This is indicative of the process required for other web servers.

### Configure a reverse proxy
1. Ensure the Apache modules mod_proxy and mod_proxy_http have been installed.
2. Open the Apache httpd.conf file and add a ‘ProxyPass’ directive to the VirtualHost element:
![alt text](Step2ConfigureReverseProxy.png "Configure Reverse Proxy")
Where:
* ‘external-server-name’ must be either the hostname of an institution, or the hostname in mandatory-config.properties.
* ‘equellahost’ is the host with the EQUELLA installation (if it is on the same machine as the apache server, this would normally be localhost).
* ‘http.port’ is the property specified in mandatory-config.properties (defaults to port 80).
* ‘nocanon’ ensures URLs are passed through to the host without processing.

An example directive is:

![alt text](ExampleDirective.png "Example Directive")

### Configure EQUELLA with SSL
1. Open mandatory-config.properties and ensure the https.port is enabled (uncommented).
2. Ensure the Apache modules mod_proxy and mod_proxy_http have been installed.
3. Open the Apache httpd.conf file and add a ‘ProxyPass’ directive to the VirtualHost element, and the additional SSL directives:

![alt text](Step3ConfigureEquellawithSSL.png "Configure with SSL")

Where:
*‘external-server-name’ must be either the hostname of an institution, or the hostname in mandatory-config.properties.
* ‘equellahost’ is the host with the EQUELLA installation (if it is on the same machine as the apache server, this would normally be localhost).
* ‘https.port’ is the property specified in mandatory-config.properties (defaults to port 8443).
* ‘nocanon’ ensures URLs are passed through to the host without processing.

An example directive is:
![alt text](ExampleDirectiveSSL.png "Example Directive SSL")

5. Update the institution URL for https://... (e.g. https://equella.com).

NOTE: The above examples are for Apache HTTPD, but hardware SSL terminators (e.g. F5 load balancer) or other software terminators (e.g. Nginx) may be used.

## Customize the EQUELLA Digital Repository

After the initial setup, EQUELLA features are managed through the Administration Console, a comprehensive tool that enables ongoing customization and administration of the EQUELLA Digital Repository.

To access EQUELLA:
1. Select the Login link for the new institution in the Institutions dialog to display the Welcome page.
2. Login to the institution using the institution administrator (TLE_ADMINISTRATOR) login and password (this is the login set when the institution was imported), then click Log In. The Dashboard page displays. 
3. To open the Administration Console, select Settings from the navigation menu on the left-hand side of the page to display the Settings page.
4. Select the Administration console link. 
5. To access the EQUELLA Digital Repository, users, roles and groups will need to be defined. 


## Use Log Files

EQUELLA writes an extensive series of log files for events including the resource center, EQUELLA services and the EQUELLA conversion service. All log files can be found in directories bearing the date of the log file. Each directory contains one log file containing entries for all events logged on that date. A new log directory is created for every day the logged services are run. Log files may need to be archived from time to time to recover disk space.

Log file Error events are highlighted to simplify discovery as the files can contain many entries. Log files are contained in the logs directory of EQUELLA server.

To view a resource centre log
1. Navigate to the logs directory, typically path-to-equella\logs\resource-centre.
2. Select the date directory and open the application.html file within.


## Thread Dump

The Thread Dump page provides information about the threads that are running, processing and being accessed. This information is used to help the system administrator identify problems. 
1. Select Thread dump from the navigation menu to display the Thread dump page. 

## Health Check

The Health check page provides system service information for services required by EQUELLA, as well as a list of currently running tasks. If clustering is configured for a system, the service and task information is provided for each cluster node.  As well as the status of each service, the following information is provided:
* Filestore – location, total space and free space.
* Image Magick – location, version, copyright information and features.
* Lucene index – index location.

Additionally, where institution filestore size restrictions have been configured, an Institution filestore usage section displays, showing the Limit and the Approximate usage. 

1. To access the Health check page from the navigation menu, select Health check. The Health check page displays. 
2. Click the Services drop-down arrow to view the service details. The relevant Node ID displays beside each running task in the Running tasks section for clustered environments.
3. If there is a problem with one or more of the services for a node, a red cross icon displays in the Services column instead of the green tick icon, Click the down-arrow to view details of the error.
4. Select the Enable cluster debugging checkbox to display cluster node information at the bottom of each EQUELLA page. 

## Uninstall EQUELLA 

This section describes how to uninstall EQUELLA. The re-installation process is the same as the installation process.

Uninstalling EQUELLA is a three-stage process on Windows and two-stage process on other platforms:
1. Stop the EQUELLA service.
2. Windows only: Run the Unregister service command to deregister the services manager.
3. Delete the directory that holds the EQUELLA installation.

### Stop the EQUELLA server
Using Windows
1. Navigate to the Start menu and find Settings, then Control Panel. 
2. Open the Administrative Tools panel and then Services. 
3. Find EQUELLA in the list of services  and select the Stop the service link on the left. 

On other platforms
1. Navigate to the Manager folder (the default installation folder is /usr/local/equella), open a command prompt and enter:
```
./manager stop
./equellaserver stop
```
The services have now stopped. 

### De-register the services manager (Windows only)

The Windows service must be removed before deleting the installation.
1. Choose Run from the Start menu then enter:
```
<path-to-equella>\manager\manager remove
<path-to-equella>\manager\equellaserver remove
```
Now the installation directory can be deleted.

### Delete the installation directory
To delete the installation directory:
1. Navigate to and select the installation directory for your installation. (The default installation is C:\equella.)
2. Press the Delete key.
3. Confirm the deletion. The directory and all its contents are deleted.
4. Ensure the database administrator deletes the EQUELLA database.


