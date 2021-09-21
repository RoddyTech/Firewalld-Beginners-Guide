
<h1 align="center">
  <br>
  <img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fcdn2.iconfinder.com%2Fdata%2Ficons%2Freal-flat-security%2F512%2Ffirewall-512.png&f=1&nofb=1" width="200" height="200"/>
  <br>
  A Beginner's Guide to Firewalld
  <br>
</h1>

## Introduction :fire:

Every device that connects to the public network must have a unique hardware address (a MAC address) before it can send and receive data in a network. Depending on the size of the network, there might be hundreds of devices, each requiring its own set of rules. For which traffic is allowed and not allowed and here's when firewalld comes into the picture

### Prerequisites

* A linux machine with Firewalld installed :desktop_computer:

## Getting Started :sparkles:

Firewalld is a dynamically managed firewall. It allows the user to set up and configure multiple firewall type options, which allows the user to block or allow incoming or outgoing traffic. By modifying firewall rules to allow and block traffic of the user's choosing.

Firewalld is a bit more complex than UFW(Uncomplicated Firewall), but provides greater flexibility and does not cause a disruption in running services when a user is managing firewall updates.

* Zones are the organization of rules of Firewalld. Each zone can contain several rules for different connection zones. A zone is associated with at least one network interface (eth0, for example). We see the preconfigured zones by using the following command: 

sudo firewall-cmd --list-all-zones: 
```
block
dmz
drop
external
home
internal
libvirt
public
trusted
work
```

* Through this division of zones, firewalld can manage rule sets dynamically without breaking existing sessions, disrupting services and bringing down the entire network.
Meaning that Rules and configurations can be tested and evaluated in runtime environments. 

* Runtime configurations are only valid until the next system reboot or service reload. This means we can create settings that are active for a limited amount of time. Runtime configurations can also be used to test new configurations. They can then be seamlessly saved to permanent environment if they work well. 

* Permanent configurations are loaded with each reboot. These become the active runtime environment until new runtime configurations are made. 

Different interfaces may requires different firewall rules due to the various services that may be in use.

* In addition to zones, firewalld uses **services** as shortcuts to configuring firewall rules for common services.

* firewalld enables you to designate which services to allow, and automatically opens the ports associated to those services. 

* For example, if you enable the SSH service in a zone, firewalld opens port `22` without requiring you to specify the port number explicitly.

* Services can be predefined, making it easy to configure the firewall rules most commonly required by most servers. Services can also be custom defined.

## TUTORIAL

