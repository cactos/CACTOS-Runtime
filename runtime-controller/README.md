# runtime-controller

A node.js based backend service run steer the data centre and control its configuration.

## Installation

This component is stand alone. It can be installed without any other component of the CACTOS Runtime Toolkit available. 
Nevertheless, in order to make it work properly and have its configuration take effect on the runtime system, several 
other tools need to be installed (see below).

The installation and operation has been tested on CentOS 7. Also, the application requires ```bash``` to be installed
on the operating system as well as ```sed``` and ```grep```.

### installing node js

```
yum install epel-release
yum install nodejs npm
```

### create runtime directory

Pick any directory on the server /runtime controller/ is supposed to run and create it ```mkdir -p /opt/rtc```. 
Then, move there: ```cd /opt/rtc```. Next, clone this repo:

```
git clone https://github.com/cactos/runtime-controller.git
cd ./runtime-controller/src
```

### installing node modules

```
npm install httpdispatcher
npm install shelljs
```

## Configuration Options

The runtime controller supports multiple configuration options that are used either by itself or the the
[runtime user interface](https://github.com/cactos/runtime-user-interface).

### Port Number

The port number used by the application is configurable in the ```server.js``` file. Update the folllowing 
line to the desired number and the ```SERVERNAME``` to the bind address.
```
//Lets define a port we want to listen to
const PORT=8080; 
const SERVERNAME="localhost";
```

###  Enabled Features
The file ```global.js``` contains switches to turn on/off features of the user interface.
```
var config = {
        'cactoopt'      : { "status" : "on" },
        'monitors'      : { "status" : "on" },
        'apps'          : { "status" : "on" },
        'metrics'       : { "status" : "on" },
    };
```
Setting the respective values to anything different from ```on``` will disable the feature in the user interface.

### Application-specific Configuration

When Runtime Controller is supposed to be used for deploying new applications, ```colosseum.js``` provides the 
necessary configuration options:
```
        'colosseumIp' : '',
        'colosseumPort' : '9000',
        'colosseumUser' : 'john.doe@example.com',
        'colosseumTenant' : 'admin',
        'colosseumPassword' : 'admin',
        'defaultCloudId' : "1",
        'defaultLocationId' : "2",
        'defaultCactosTenantName' : "RegionOne/cactos-testing",
```

It is mandatory to fill-in ```colosseumIp```. This as well as ``colosseumPort``` as well as the other colosseum 
properties depend on the configuration of the [Runtime Toolkit](http://#).

### CactoOpt-specific Configuration 

When Runtime Controller is supposed to be used for controlling the CactoOpt configuration, ```opt_options.js``` 
provides a single configuration options:
```

const optConfigDir = "/tmp/eu.cactosfp7.configuration/";

```
Change the value of ```optConfigDir``` to have it point ot the installation location of the configuratoin files for 
your [Runtime Toolkit](http://#). In case you are planning to use this feature, make sure that the file
```config_operator.sh``` shipping with the runtime controller is in the ```bin``` directory and can be executed by
the user under which node.js executes. You can make it executable by ```chmode +x /path/to/file/config_operator.sh```.

## Starting the Service

For a simple test, enter the installation directory (e.g., ```cd /opt/rtc``` and then further 
to ```cd ./runtime-controller/src```). There, run node.js:
```
node server.js
```

### Running as a Daemon Service
TODO: run as daemon service
