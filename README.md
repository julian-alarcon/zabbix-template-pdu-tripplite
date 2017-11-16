# Zabbix Template for Tripp Lite PDUs

    Author: Julian Alarcon
    Language: XML
    License: MIT

This template provides monitoring of Tripp Lite PDUs using Zabbix. It may work for other manufacturers.
It was tested on the PDU with reference PDUMNV20 of Tripp Lite.(https://www.tripplite.com/1.9kw-single-phase-monitored-pdu-120v-outlets-24-5-15-20r-l5-20p-5-20p-adapter-0u-vertical-70-in-taa~PDUMNV20/)

The firmware used on the PDU was version 12.06.0069 of 2016-02-08.

Minimum version of Zabbix is 3.0.
It uses the Zabbix Core SNMP Templates 	Template SNMP Interfaces and Template SNMP Generic.

This is based in 2 files, one from Zabbix Share and another from the forum plus information from other places.

There is no need to import the MIBs files from Tripp Lite, as I'm using complete full OID numbers, but these are available in the Sources section.

## Monitored values from Device

This template allows to monitor:

* Device Model
* Device Agent Software Version
* Device Input Line Index
* Device Input Lines
* Device Software Version
* Device Name
* Attached Devices
* Device Manufacturer
* Device Alarm
* Device Input Frequency
* Device Input Voltage
* Device Output Current
* Device Output Source
* Device Number of Output Lines
* Device Output Line Index

## Available triggers (Alarms)

* PDU Alarm Present (Warning)
* PDU Current Output Too High (Warning)
* PDU Current Output Reaching Limit (High)
* PDU Output Source Not Normal (High)
* PDU Power Voltage Too High (High)
* PDU Power Voltage Too Low (High)

## Available Graphs

* Output Current
* Input Frecuency
* Input Voltage

## Variable values (Macros)

You are able of set personal values using the specific macros values for your devices. (https://www.zabbix.com/documentation/3.0/manual/config/macros/usermacros)

This is the list of available macros and their default values:

* Name: `{$SNMP_COMMUNITY}`

  Default value: `tripplite`

  Description: Value of SNMP community, if you change your default community in the PDU you must setup the define in Zabbix the new value for your device.

* Name: `{$MAX_INPUTVOLTAGE}`

  Default value: `125`

  Description: If the Input Voltage is above this value, you will get an alarm of severity High from Zabbix.

* Name: `{$MIN_INPUTVOLTAGE}`

  Default value: `115`

  Description: If the Input Voltage is below this value, you will get an alarm of severity High from Zabbix.

* Name: `{$MAX_OUTPUTCURRENT_WARNING}`

  Default value: `15`

  Description: If the Input Voltage is above this value, you will get an alarm of severity Warning from Zabbix.

* Name: `{$MAX_OUTPUTCURRENT_CRITICAL}`

  Default value: `18`
  
  Description: If the Input Voltage is above this value, you will get an alarm  of severity High from Zabbix.

## Static values

There are some static elements, you can download and edit the xml template to set yours.

* Maximum value in graph Output Current is 50
* Maximum value in graph Input Voltage is 240
* Maximum value in graph Input Frecuency is 80

## Frecuency and Current calculation

SNMP values returned by the PDU of the Frecuency and Current are missing one decimal point, getting values of 10 Amps instead of 1 Amp.
I used the Zabbix multiplier and function properties to show in graphs and the alarms the righ values. You can remove it if you want.

To get the original SNMP data change the next values from the template:
```
<multiplier>1</multiplier>
<formula>0.1</formula>
```
to :
```
<multiplier>0</multiplier>
<formula>1</formula>
```

## SNMP example values

```
[root@zabbixserver ~]# snmpwalk -Os -c triplite -v 2c 10.0.0.1 | grep mib
mib-2.33.1.1.1.0 = STRING: "TRIPP LITE"
mib-2.33.1.1.2.0 = STRING: "PDUMNV20"
mib-2.33.1.1.3.0 = STRING: "6926160C"
mib-2.33.1.1.4.0 = STRING: "12.06.0069"
mib-2.33.1.1.5.0 = STRING: "name-of-pdu"
mib-2.33.1.1.6.0 = STRING: ",,,,,,,,,,,,,,,,,,,,,,,"
mib-2.33.1.3.2.0 = INTEGER: 1
mib-2.33.1.3.3.1.1.1 = INTEGER: 1
mib-2.33.1.3.3.1.2.1 = INTEGER: 600
mib-2.33.1.3.3.1.3.1 = INTEGER: 119
mib-2.33.1.4.1.0 = INTEGER: 3
mib-2.33.1.4.3.0 = INTEGER: 1
mib-2.33.1.4.4.1.1.1 = INTEGER: 1
mib-2.33.1.4.4.1.3.1 = INTEGER: 40
mib-2.33.1.6.1.0 = INTEGER: 0
mib-2.33.1.9.1.0 = INTEGER: 120
mib-2.33.1.9.9.0 = INTEGER: 70
```

mib-2 correspond to the OID: .1.3.6.1.2.1

# Sources

* https://www.tripplite.com/support/SNMPWEBCARD
* https://www.zabbix.com/forum/showthread.php?t=53910
* https://www.zabbix.com/forum/attachment.php?attachmentid=9129&d=1466024491
* https://share.zabbix.com/power-ups/tripplite-ups
* http://pydoc.net/pysnmp-mibs/0.1.6/pysnmp_mibs.UPS-MIB/
* https://www.zabbix.com/documentation/3.0/manual/config/items/item