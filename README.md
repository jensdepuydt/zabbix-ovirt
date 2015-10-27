# zabbix-ovirt
This is a template which can be used to monitor oVirt or libvirt virtualization hosts from within Zabbix.

The template was exported from Zabbix version 2.4.3
Tested with oVirt version 3.5.0.1 release 1.el6, libvirt version 0.10.2 release 46.el6_6.2 on CentOS 6.6
Tester with oVirt version 3.5.5 on CentOS 7.1 with systemd service files

## How to use:
1. Enable SNMP on the oVirt or libvirt host
  - give direct access to libvirt for root (disabled by default for oVirt)
  - install and configure basic SNMP
  - install libvirt-snmp
  - create init-script or systemd service for libvirt-snmp
  - start and test the service

see my detailed blogpost for instructions on how to enable SNMP for oVirt or libvirt:
[Monitor oVirt or libvirt with SNMP and Zabbix](http://jensd.be/?p=494)

2. Create a new value mapping in Zabbix:
  - On the Zabbix-server, go to: Afministration -> General -> Value Mapping (upper right corner)
  - Create value map
  - name: libvirt guest state
  - mappings: 
````
1 -> running
2 -> blocked
3 -> paused
4 -> shutdown
5 -> shutoff
6 -> crashed
````
	
3. Import the template in Zabbix:
  - On the Zabbix-server, go to Configuration - Templates
  - click on Import
  - choose the file and click on Import
	
4. Add your oVirt or libvirt hosts to Zabbix:
  - On the Zabbix-server, go to Configuration - Hosts
  - for each host: click on Create Host
  - On the host tab:
  - enter a host name 
    - add an SNMP-interface with your ovirt or libvirt host's IP and/or hostname
  - On the templates tab:
    - add template "Template SNMP oVirt"
  - On the Macros tab:
    - enter macro {$SNMP_COMMUNITY} with <community> as value
    - enter macro {$SNMP_PORT} with your SNMP-port as value (default 161)
  - click Save

5. Wait for the discovery to fire that creates all items, graphs and triggers
  - by default, this takes 3600s or one hour :)
