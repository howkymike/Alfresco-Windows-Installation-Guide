# Alfresco-Windows-Installation-Guide
Step-by-step Alfresco 6.x non containerized installation guide on Windows

### Prerequisites
 - **Java 8** installed & set up JAVA_HOME
 - Up and running Database
	Steps to initialize **PostgreSQL**:
	- Download [PostrgreSQL]([https://www.postgresql.org/download/]) and install (pref on port 5432)
	- Using [pgAdmin](https://www.pgadmin.org/) create database 'alfresco' with encoding 'utf8'
	- create role alfresco login and password both: 'alfresco'
	- grant all on database alfresco to alfresco;
	You can do it also via command line by entering these commands:
	`CREATE USER alfresco WITH PASSWORD 'admin';`
	`CREATE DATABASE alfresco OWNER alfresco ENCODING 'utf8';`
	`GRANT ALL PRIVILEGES ON DATABASE alfresco TO alfresco;`
- Up and ready to run **Tomcat** (tested on v.9)
		- Download & extract [Tomcat](https://tomcat.apache.org/download-90.cgi) 
-  **Alfresco** files
		- Download & extract [Alfresco files](https://www.alfresco.com/thank-you/thank-you-downloading-alfresco-community-edition) (Distribution and  Alfresco Search Services)
	
	
### Installation
1. Create 'Alfresco' folder (pref: `C:\Alfresco`). It will be called <ALFRESCO_ROOT>
2. Copy Tomcat folder to <ALFRESCO_ROOT>. It will be called <TOMCAT_HOME>
3. Create following directories under <TOMCAT_HOME>
	- shared/classes
	- shared/lib
4.	Create following directories in <ALFRESCO_HOME>
	- modules/platform
	- modules/share
5. Modify <TOMCAT_HOME>/conf/catalina.properties 
	- shared.loader=${catalina.base}/shared/classes
6. Extract Distribution.zip
	- Copy "alf_data", "alfresco-pdf-renderer", "amps", "bin", "licenses" into <ALFRESCO_HOME>
	- Copy all web-server\*  folders to <TOMCAT_HOME>/
	- Remove all directories (**only** directories)  from  <TOMCAT_HOME>/webapps
7. Create `alfresco-global.properties` file in <TOMCAT_HOME>/shared/classes and paste (This is only a basic template. You can edit it and make own custom settings):
```
#
# Custom content and index data location
#
dir.root=C:/Alfresco/alf_data
dir.keystore=${dir.root}/keystore

#
# Sample database connection properties
#
db.username=alfresco
db.password=alfresco

# PostgreSQL
db.driver=org.postgresql.Driver
db.url=jdbc:postgresql://localhost:5432/alfresco

# MySQL
#db.driver=com.mysql.jdbc.Driver
#db.url=jdbc:mysql://${db.host}:${db.port}/${db.name}?useUnicode=yes&characterEncoding=UTF-8 

# Other databases configurations:
# https://docs.alfresco.com/6.1/concepts/intro-db-setup.html

# Email configurations:
# https://docs.alfresco.com/6.1/concepts/email-intro.html

#
# URL Generation Parameters (The ${localname} token is replaced by the local server name)
#-------------
alfresco.context=alfresco
alfresco.host=${localname}
alfresco.port=8080
alfresco.protocol=http

share.context=share
share.host=${localname}
share.port=8080
share.protocol=http

# Solr6
index.subsystem.name=solr6
solr.secureComms=none
solr.host=localhost
solr.port=8983
solr.port.ssl=8984

# ssl encryption
#encryption.ssl.keystore.location=${dir.keystore}/ssl.keystore
#encryption.ssl.keystore.type=JCEKS
#encryption.ssl.keystore.keyMetaData.location=${dir.keystore}/ssl-keystore-passwords.properties
#encryption.ssl.truststore.location=${dir.keystore}/ssl.truststore
#encryption.ssl.truststore.type=JCEKS
#encryption.ssl.truststore.keyMetaData.location=${dir.keystore}/ssl-truststore-passwords.properties

# secret key keystore configuration
#encryption.keystore.location=${dir.keystore}/keystore
#encryption.keystore.keyMetaData.location=${dir.keystore}/keystore-passwords.properties
#encryption.keystore.type=JCEKS


# If not using ActiveMQ
messaging.subsystem.autoStart=false

# When using CMIS in browser
#alfresco.restApi.basicAuthScheme=true

# ImageMagick
#img.root=C:\\Alfresco\\ImageMagick
#img.exe=${img.root}\\convert.exe
#img.coders=${img.root}/modules/coders
#img.config=${img.root}/config

# pdf-renderer
alfresco-pdf-renderer.root=c:/Alfresco/alfresco-pdf-renderer
alfresco-pdf-renderer.exe=${alfresco-pdf-renderer.root}/alfresco-pdf-renderer

# LibreOffice
#jodconverter.officeHome=c:/Alfresco/LibreOffice
#jodconverter.portNumbers=8101




```
__It is highly recommended to at least look through this configuration file.__

8.  Alfresco-search-services 
	- Extract and copy to <ALFRESCO_ROOT>. You can rename it to `solr6`
	- Update the solr6/solr.in.cmd
		`set SOLR_ALFRESCO_PORT=8983`
	- Start solr in cmd <-- It eneables many Alfresco features. You should have it started before starting Alfresco.
	`./solr/bin/solr start -a "-Dcreate.alfresco.defaults=alfresco,archive"`

	You can stop it by enterning this command:
	`./solr/bin/solr stop`
	
	- Verify if you can enter http://localhost:8983/solr/#/
9.  Start Alfresco by executing  <TOMCAT_HOME>/bin/startup.bat
10. Enjoy!


#### Detailed instructions:
[Alfresco installation guide](https://docs.alfresco.com/community/concepts/install-community-intro.html)

