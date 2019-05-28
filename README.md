## Docker project template for PHP7.3, Apache2, MySQL, PhpMyAdmin and Xdebug

This is a docker project to create a container with PHP7.3, Apache2, MySQL, PhpMyAdmin and Xdebug.


    docker-compose up

Starts the container named php7.3-apache2

The following table show the credentials for MySQL and PhpMyAdmin.

Service | User      | Password
---     | ---       | ---
MySQL   | root      | password | 
PHP     | root      | password | 

See Dockerfile and [docker-compose.yml](docker-compose.yml) for configuration details. You can change the credentials in the file named.


Service     | Version   | Port
---         | ---       | ---
Apache2     | 2.4.25    | 80   | 
PHP         | 7.3.4     | 80   | 
Xdebug      | 2.6.0     | 9000 |
MySQL       | 5.7       | 3600 |
PhpMyAdmin  | 4.6.0     | 8080 |

Xdebug runs in port 9000 

Visual Studio Code launch.json for activate xdebug:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for XDebug",
            "type": "php",
            "request": "launch",
            "pathMappings": {
               "/var/www/html": "${workspaceRoot}/htdocs",
            },
            "port": 9000,
            "log": true
        }
    ]
}
```

To enter into mysql command line interface **in the host (yours) terminal** enter:

```bash
mysql --host=127.0.0.1 --user=root --password=password
```

If you want to check the databases in your mysql server:
```sql
mysql> show databases;
```
For create a new database and import it into your mysql:
```sql
mysql> create database __YOUR_DATABASE_NAME__;
mysql> use __YOUR_DATABASE_NAME__;
mysql> source __PATH_TO_YOUR_DATABASE__;
```

To change the domain name server to the project need to change the **ServerName** from the **sites/httpd.conf** for whatever you need.
```apacheconf
<VirtualHost *:80>

  ServerName template.me
  DocumentRoot "/var/www/html"
 
</VirtualHost>
```

 Don't forget to add the domain added to your hosts file located in **/etc/hosts**
```
 vim /etc/hosts
 ```
If you want access to PhpMyAdmin, go to your domain pointing to port 8080:

>   http://[YOUR_DOMAIN]:8080

by default you can enter doing:

>   http://template.me:8080

## Troubleshooting

If you have the following problem after try to *docker-compose up*
```
λ pop-os docker-template → docker-compose up 
Creating network "service_name_default" with the default driver
Creating service_name_database_1 ... error

ERROR: for service_name_default_1  Cannot start service database: driver failed programming external connectivity on endpoint service_name_database_1 (ee5cf2c8423ab4a20694ef0f8a1320b90ac425b4e214479f8f88a5c0331e60ad): Bind for 0.0.0.0:3306 failed: port is already allocated
```
Means that you have already allocated the ports that you are trying to use. Now you can simply do a:
```
λ pop-os docker-template → sudo netstat -ntlp
[sudo] password for fantzuaj: 
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:33065           0.0.0.0:*               LISTEN      4165/spotify        
tcp        0      0 127.0.0.1:45357         0.0.0.0:*               LISTEN      17019/code          
tcp        0      0 0.0.0.0:57621           0.0.0.0:*               LISTEN      4165/spotify        
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      1044/systemd-resolv 
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      1421/cupsd          
tcp6       0      0 ::1:631                 :::*                    LISTEN      1421/cupsd          
```
check for the port that are already in use and kill it / them (you have to use the PID):
```
sudo kill [PID]
```
where PID is the process identifier that is associated with each connection to identify port conflicts. For instance if you would like to kill the port that is using spotify, PID would be the same 4165. You should do:
```
sudo kill 4165
```