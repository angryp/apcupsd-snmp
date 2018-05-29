# Net-SNMP module for apcupsd

This is Net-SNMP module for monitoring APC UPSes without SNMP support. It reads
output from apcupsd (/sbin/apcaccess) and writes it into appropriate OIDs like
UPSes with built-in SNMP support.


## Installation
 
To load this into a running agent with embedded Perl support turned on, simply 
put the following line to your snmpd.conf file:

	perl do "/path/to/mod_apcupsd.pl";

Net-snmp must be compiled with Perl support and apcupsd properly configured 
and running!


## Use

Try `snmpwalk -v 2c -c public <host> .1.3.6.1.4.1.318.1.1.1` and you should
get something like:

	$ snmpwalk -v 2c -c public localhost .1.3.6.1.4.1.318.1.1.1
	PowerNet-MIB::upsBasicIdentName.0 = STRING: "secretstring"
	PowerNet-MIB::upsAdvIdentFirmwareRevision.0 = STRING: "468.00.I"
	PowerNet-MIB::upsAdvIdentSerialNumber.0 = STRING: "secretstring"
	PowerNet-MIB::upsBasicBatteryTimeOnBattery.0 = Timeticks: (0) 0:00:00.00
	PowerNet-MIB::upsBasicBatteryLastReplaceDate.0 = STRING: "08/15/14"
	PowerNet-MIB::upsAdvBatteryCapacity.0 = Gauge32: 100
	PowerNet-MIB::upsAdvBatteryTemperature.0 = Gauge32: 20
	PowerNet-MIB::upsAdvBatteryRunTimeRemaining.0 = Timeticks: (192000) 0:32:00.00
	PowerNet-MIB::upsAdvBatteryNominalVoltage.0 = INTEGER: 240
	PowerNet-MIB::upsAdvBatteryActualVoltage.0 = INTEGER: 275
	PowerNet-MIB::upsAdvInputLineVoltage.0 = Gauge32: 220
	PowerNet-MIB::upsAdvInputFrequency.0 = Gauge32: 50
	PowerNet-MIB::upsAdvInputLineFailCause.0 = INTEGER: largeMomentarySpike(8)
	PowerNet-MIB::upsAdvOutputVoltage.0 = Gauge32: 229
	PowerNet-MIB::upsHighPrecOutputLoad.0 = Gauge32: 230
	PowerNet-MIB::upsAdvConfigRatedOutputVoltage.0 = INTEGER: 230
	PowerNet-MIB::upsAdvConfigHighTransferVolt.0 = INTEGER: 265
	PowerNet-MIB::upsAdvConfigLowTransferVolt.0 = INTEGER: 196
	PowerNet-MIB::upsAdvConfigAlarm.0 = INTEGER: 0
	PowerNet-MIB::upsAdvConfigMinReturnCapacity.0 = INTEGER: 0
	PowerNet-MIB::upsAdvConfigSensitivity.0 = INTEGER: high(4)
	PowerNet-MIB::upsAdvConfigLowBatteryRunTime.0 = Timeticks: (0) 0:00:00.00
	PowerNet-MIB::upsAdvConfigReturnDelay.0 = Timeticks: (0) 0:00:00.00
	PowerNet-MIB::upsAdvConfigShutoffDelay.0 = Timeticks: (96000) 0:16:00.00
	PowerNet-MIB::upsAdvTestDiagnosticSchedule.0 = INTEGER: biweekly(2)
	PowerNet-MIB::upsAdvTestDiagnosticsResults.0 = INTEGER: 0

or if you like numeric OIDs:

	$ snmpwalk -v 2c -c public localhost .1.3.6.1.4.1.318.1.1.1
	.1.3.6.1.4.1.318.1.1.1.1.1.2.0 = STRING: "secretstring"
	.1.3.6.1.4.1.318.1.1.1.1.2.1.0 = STRING: "468.00.I"
	.1.3.6.1.4.1.318.1.1.1.1.2.3.0 = STRING: "secretstring"
	.1.3.6.1.4.1.318.1.1.1.2.1.2.0 = Timeticks: (0) 0:00:00.00
	.1.3.6.1.4.1.318.1.1.1.2.1.3.0 = STRING: "08/15/14"
	.1.3.6.1.4.1.318.1.1.1.2.2.1.0 = Gauge32: 100
	.1.3.6.1.4.1.318.1.1.1.2.2.2.0 = Gauge32: 20
	.1.3.6.1.4.1.318.1.1.1.2.2.3.0 = Timeticks: (192000) 0:32:00.00
	.1.3.6.1.4.1.318.1.1.1.2.2.7.0 = INTEGER: 240
	.1.3.6.1.4.1.318.1.1.1.2.2.8.0 = INTEGER: 275
	.1.3.6.1.4.1.318.1.1.1.3.2.1.0 = Gauge32: 220
	.1.3.6.1.4.1.318.1.1.1.3.2.4.0 = Gauge32: 50
	.1.3.6.1.4.1.318.1.1.1.3.2.5.0 = INTEGER: largeMomentarySpike(8)
	.1.3.6.1.4.1.318.1.1.1.4.2.1.0 = Gauge32: 229
	.1.3.6.1.4.1.318.1.1.1.4.3.3.0 = Gauge32: 230
	.1.3.6.1.4.1.318.1.1.1.5.2.1.0 = INTEGER: 230
	.1.3.6.1.4.1.318.1.1.1.5.2.2.0 = INTEGER: 265
	.1.3.6.1.4.1.318.1.1.1.5.2.3.0 = INTEGER: 196
	.1.3.6.1.4.1.318.1.1.1.5.2.4.0 = INTEGER: 0
	.1.3.6.1.4.1.318.1.1.1.5.2.6.0 = INTEGER: 0
	.1.3.6.1.4.1.318.1.1.1.5.2.7.0 = INTEGER: high(4)
	.1.3.6.1.4.1.318.1.1.1.5.2.8.0 = Timeticks: (0) 0:00:00.00
	.1.3.6.1.4.1.318.1.1.1.5.2.9.0 = Timeticks: (0) 0:00:00.00
	.1.3.6.1.4.1.318.1.1.1.5.2.10.0 = Timeticks: (96000) 0:16:00.00
	.1.3.6.1.4.1.318.1.1.1.7.2.1.0 = INTEGER: biweekly(2)
	.1.3.6.1.4.1.318.1.1.1.7.2.3.0 = INTEGER: 0

You can also query only one OID:

	$ snmpwalk -v 2c -c public grid .1.3.6.1.4.1.318.1.1.1.2.2.3.0
	PowerNet-MIB::upsAdvBatteryRunTimeRemaining.0 = Timeticks: (190200) 0:31:42.00
	

## What can be improved

* Reimplement snmp_handler to correctly support walking through subtrees of 
.1.3.6.1.4.1.318.1.1.1 (e.g. .1.3.6.1.4.1.318.1.1.1.2). Currently it can 
list subtrees only on .1.3.6.1.4.1.318.1.1.1 and leafs.

* Add remaining OIDs that apcupsd could get data for. I included only OIDs for 
my APC Back-UP RS 500.

* Implement support for setting values and traps.

*Feel free to contribute!* I have no intention in future development.


## Important notes

* Download PowerNet (APC) MIB file: 
[powernet401.mib](http://www.michaelfmcnamara.com/files/mibs/powernet401.mib).

* Great thank you to @jirutka for initial script, it really helped for general
purpose, but very little adjustments should have been done for Smart-UPS RC 10000.
