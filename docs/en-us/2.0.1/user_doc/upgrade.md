
# DolphinScheduler upgrade documentation

## 1. Back Up Previous Version's Files and Database.

## 2. Stop All Services of DolphinScheduler.

 `sh ./script/stop-all.sh`

## 3. Download the New Version's Installation Package.

- [Download](/en-us/download/download.html) the latest version of the installation packages.
- The following upgrade operations need to be performed in the new version's directory.

## 4. Database Upgrade
- Modify the following properties in `conf/config/install_config.conf`.

- If you use MySQL as the database to run DolphinScheduler, please comment out PostgreSQL related configurations, and add mysql connector jar into lib dir, here we download mysql-connector-java-8.0.16.jar, and then correctly config database connect information. You can download mysql connector jar [here](https://downloads.MySQL.com/archives/c-j/). Alternatively, if you use Postgres as database, you just need to comment out Mysql related configurations, and correctly config database connect information.

```conf
# Database type, username, password, IP, port, metadata. For now dbtype supports `mysql` and `postgresql`, `H2`
# Please make sure that the value of configuration is quoted in double quotation marks, otherwise may not take effect
DATABASE_TYPE="mysql"
SPRING_DATASOURCE_URL="jdbc:mysql://ds1:3306/ds_201_doc?useUnicode=true&characterEncoding=UTF-8"
# Have to modify if you are not using dolphinscheduler/dolphinscheduler as your username and password
SPRING_DATASOURCE_USERNAME="dolphinscheduler"
SPRING_DATASOURCE_PASSWORD="dolphinscheduler"
```

- Execute database upgrade script

    `sh ./script/create-dolphinscheduler.sh`

## 5. Backend Service Upgrade.

### 5.1 Modify the Content in `conf/config/install_config.conf` File.
- Standalone Deployment please refer the [6, Modify running arguments] in [Standalone-Deployment](/en-us/docs/2.0.1/user_doc/guide/installation/standalone.html).
- Cluster Deployment please refer the [6, Modify running arguments] in [Cluster-Deployment](/en-us/docs/2.0.1/user_doc/guide/installation/cluster.html).

#### Masters Need Attentions

1、Modify the workers config item in conf/config/install_config.conf file.

Imaging bellow are the machine worker service to be deployed:
| hostname | ip |
| :---  | :---:  |
| ds1   | 192.168.xx.10     |
| ds2   | 192.168.xx.11     |
| ds3   | 192.168.xx.12     |

To keep worker group config consistent with the previous version, we need to modify workers config item as below:

```shell
#worker service is deployed on which machine, and also specify which worker group this worker belongs to. 
workers="ds1:service1,ds2:service2,ds3:service2"
```

### 5.2 Execute Deploy Script.
```shell
`sh install.sh`
```


