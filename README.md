# isc-dhcp-subnet

A BASH script used to automate the process of adding a subnet block and interface
to the isc-dhcp-server configuration.


# usage

isc-dhcp-subnet.sh - Create or remove a subnet configuration for isc-dhcp-server.

Used to modify the dhcpd configuration file with a subnet address block
and the interface settings in the default isc-dhcp-server configuration file.

> isc-dhcp-subnet.sh command subnet [interface]
- command - the command to execute, create, remove
- subnet - the subnet address to use for the dhcp block
- interface - the interface that will be used for dhcp service

## command

Call the script with the command to be executed. Use *create* to create a
new subnet block add the interface if provided. Or use *remove* to
remove a subnet block and remove the interface if provided.

## subnet

Specify the subnet to be added or removed, i.e. *192.168.18.0*.  
NOTE: A netmask of 255.255.255.0 is assumed.
NOTE: The script is configured to use the IP range of 100 to 240 for DHCP.

## interface

The interface is optional. If an interface name is provided then it will be
added or removed from the isc-dhcp-server default interface settings.


# example 1

Creating a DHCP subnet 192.168.99.0 on interface eth1...

> sudo ./isc-dhcp-subnet.sh create 192.168.99.0 eth1

Resulting dhcp subnet block in dhcpd.conf...

```
#### START 192.168.99.0 DO NOT EDIT ####
subnet 192.168.99.0 netmask 255.255.255.0 {
  range 192.168.99.100 192.168.99.240;
  option subnet-mask 255.255.255.0;
}
#### END 192.168.99.0 DO NOT EDIT ####
```

Resulting interfaces settings in the defaults isc-dhcp-server...

```
INTERFACESv4="eth1"
```


# example 2

Remove a DHCP subnet block but keep the interface setting for isc-dhcp-server...

> sudo ./isc-dhcp-server.sh remove 192.168.99.0

The subnet block for 192.168.99.0 will be removed from the dhcpd.conf file
but the interface in the isc-dhcp-server default settings will remain.
