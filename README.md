# initIOC

Script for generating camera IOCs from the [ioc-template](https://github.com/epicsNSLS2-deploy/ioc-template).

### Usage

This script is intended for rapidly initializing IOCs for new detectors en-mass. To use the script, you will need to edit the `CONFIGURE` file in this directory. Within this file there is a table with several example IOCs included.
```
# IOC Type    IOC Name      AsynPort      IOC Port     Cam Connection
#-------------------------------------------------------------------
ADUVC         cam-uvc        UVC1           4000          25344
ADProsilica   cam-ps1        PS1            4001       EX.AM.PLE.IP
ADAndor3      cam-andor3     AD3            4002       /exam/ple/path
```
This table contains all of the IOCs that will be autogenerated by the script. Please follow these rules when adding IOCs to the table:

Parameter | Description
--------|------------------
IOC Type | Should be the same as its areaDetector repository name. `ADProsilica`, `ADPointGrey` etc. Note that `AD` must be at the start of this parameter
IOC Name | Name of the IOC to be deployed. Usually `cam-` followed by a camera type and number. ex `cam-uvc1`
Asyn Port | Name of the asyn port to which the NDArrays from the driver will be piped. Usually an all-caps shorthand of the driver name followed by a number
IOC Port | The telnet port that the IOC will run on when started by procServer
Cam Connection | A general parameter for whatever connection format the camera uses. Can be serial number, config path, IP etc.

Other configuration options that are found in `CONFIGURE` include:  

Option | Description
---------|--------
IOC_DIR | Location for iocs on the server. Usually `/epics/iocs`
TOP_BINARY_DIR | Location of compiled epics base and support
BINARIES_FLAT | Toggle that tells the script whether base and support are separated or if support modules are in the same directory as base. If using a prebuilt bundle, this will most likely need to be set to YES
ENGINEER | The engineer deploying the IOC
PREFIX | IOC PV Prefix for the camera. - Note that the script will attempt to autoassign a unique portion of the prefix
HOSTNAME | Hostname of server on which the IOC is located
CA_ADDRESS | The channel access address list IP

Once all of these configuration options are set and the IOCs are added to the table, simply run
```
./initIOCs.py
```
in the top level of this directory. **(Note that python3 is required for the script to run)**

You may also utilize certain optional command line flags with initIOC:

Option | Description
------|------------
-h / --help | Prints help information
-i / --individual | Gives you a guided process for initializing IOCs one at a time
-g / --gui | Starts the GUI version of initIOC - will be available in later versions

### Currently supported drivers

initIOCs.py relies on [ioc-template](https://github.com/epicsNSLS2-deploy/ioc-template) to deploy its IOCs, and as a result, IOC support is limited to those drivers that have startup scripts located in `ioc-template`. Currently this includes:
* ADAndor3
* ADLambda
* ADUVC
* ADProsilica
* ADPilatus
* ADPerkinElmer
* ADMerlin
* ADSpinnaker
* ADPointGrey
* ADSimDetector
* ADURL