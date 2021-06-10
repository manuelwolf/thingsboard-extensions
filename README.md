Extension ThingsBoard Platform
=====================

## Run project in development mode
 
```
cd ${TB_EXTENSION_WORK_DIR}/widgets
mvn clean install -P npm-start
```

In widgets library create a new widget. In the resources tab of the widget editor add this file:
```
static/widgets/thingsboard-extension-widgets.js
```

You also need to update the local UI TB settings. You need to replace the proxy settings in the file ui-ngx/proxy.conf.js with:
```
"/static/widgets": {
    "target": "http://localhost:5000",
    "secure": false,
}
```

## Run project in production mode

In widgets library create a new widget. In the resources tab of the widget editor add this file:
```
static/widgets/thingsboard-extension-widgets.js
```

## Build project

```
cd ${TB_EXTENSION_WORK_DIR}
mvn clean install -DskipTests
```

## Deploy project to customer server

```
cd ${TB_EXTENSION_WORK_DIR}
scp widgets/target/thingsboard-extension-widgets-1.0.0-SNAPSHOT.jar ubuntu@${CUSTOMER}:~/.
ssh ${CUSTOMER}

sudo cp thingsboard-extension-widgets-1.0.0-SNAPSHOT.jar /usr/share/thingsboard/extensions/
sudo chown thingsboard:thingsboard /usr/share/thingsboard/extensions/thingsboard-extension-widgets-1.0.0-SNAPSHOT.jar
sudo service thingsboard restart
```

## Build docker image with custom extension
Before building the docker image you have to choose the proper TB version, by default it has been set to latest 
ThingsBoard CE.
<br>
An example of setting version:
<br>
CE:
```
thingsboard/tb-postgres
```
PE:
```
store/thingsboard/tb-pe-node:3.2.2PE
```

To build a docker image with a custom extension inside, you need to specify the repository name, the image name and 
ThingsBoard version by executing following command:

```
mvn clean install -Ddockerfile.skip=false -Ddocker.repo=thingsboard -Ddocker.name=thingsboard-extension-docker -Dtb.edition=thingsboard/tb-postgres
```