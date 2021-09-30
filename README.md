
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

# TUTORIAL: :exclamation:

### Note the following:


* eth0 = first physical interface on a device

* eth1 = second physical interface on a device

* wlan0 = first WiFi interface

* wlan1 = second WiFi interface, etc.

### Commands:

* Use sudo /etc/init.d/firewalld start: to start the firewall.
* Use sudo firewall-cmd: to create, modify, and delete rules.
* Use --list-all-zones: to list the currently configured zones.
* Use --zone=work --change-interface=eth1: to bind zones to physical interfaces.
* Use --list-all: to list all active rules in a zone.
* Use --get-services: to list the currently configured services.
* Use --add-rich-rule=: to configure rules with more detailed options.
* Use --add-icmp-block=: to block ping requests.


### Starting firewalld:

1. Since firewalld is dynamic, we can start it and make changes without having to restart or reload it. We start firewalld using the following command:
   - sudo /etc/init.d/firewalld start
   Output should look similar to below:
   
   ``` [ ok ] Starting firewalld (via systemctl): firewalld.service. ```

### firewalld Zone Viewing
2. With firewalld, we can set a zone to an interface and configure each individual zone setting. We can use ``` firewall-cmd ``` to list zones and services, associate zones with interfaces, and also configure new firewall rules. Run the following command:

     - ``` sudo firewall-cmd --list-all-zones ```
      * firewall-cmd: Option that tells firewalld to create, modify, and delete rules.
      * --list-all-zones: Command option that lists all currently configured zones.
      
 Your output should reflect the following default zones that come preconfigured:
 
    block, dmz, drop, external, home, internal, public, trusted, and work.

### Binding Zone to Physical Interfaces
3. Since we know that the zone will be work-related and the physical interface is located on the third floor is eth1,  we need to bind the work zone to the eth1 interface.
Run the following command:

-  ``` sudo firewall-cmd --zone=work --change-interface=eth1 ```

  * --zone=work: Displays information regarding the work zone.
  * --change-interface=: Command option used to bind an interface to a different zone.
  * eth1: The interface used to change to.

Output should read:

```success```

* The process of binding ```eth1``` to the ```work``` zone causes the ```eth1``` interface to inherit all rules from the work zone.

### Zone and Service Verification
4. Let's verify the binding of the work zone to the physical interface of eth1.
Run the following command:

- sudo firewall-cmd --zone=work --list-all

- --list-all: Lists all settings for a specific zone.

Output should look similar to:
````
work (active)
target: default
icmp-block-inversion: no
interfaces: eth1
sources: 
services: ssh dhcpv6-client
ports: 
protocols: 
masquerade: no
forward-ports: 
source-ports: 
icmp-blocks: 
rich rules: 
````

- We can see that `icmp-blocks` will block `echo-reply` and `echo-request`.

- We can see which services are allowed. If the service isn't listed, it will be blocked.

   - Next we will list all currently running services inside firewalld.

   Type the following command:

    - `sudo firewall-cmd --get-services`
        - `--get-services`: Returns a list of all currently running services.

   Output should look similar to:

      ```
      RH-Satellite-6 amanda-client amanda-k5-client bacula bacula-client bgp bitcoin bitcoin-rpc bitcoin-testnet bitcoin-testnet-rpc ceph ceph-mon cfengine condor-collector ctdb dhcp dhcpv6 dhcpv6-client dns docker-registry docker-swarm dropbox-lansync elasticsearch freeipa-ldap freeipa-ldaps freeipa-replication freeipa-trust ftp ganglia-client ganglia-master git high-availability http https imap imaps ipp ipp-client ipsec irc ircs iscsi-target kadmin kerberos kibana klogin kpasswd kprop kshell ldap ldaps libvirt libvirt-tls managesieve mdns minidlna mosh mountd ms-wbt mssql murmur mysql nfs nfs3 nrpe ntp openvpn ovirt-imageio ovirt-storageconsole ovirt-vmconsole pmcd pmproxy pmwebapi pmwebapis pop3 pop3s postgresql privoxy proxy-dhcp ptp pulseaudio puppetmaster quassel radius redis rpc-bind rsh rsyncd samba samba-client sane sip sips smtp smtp-submission smtps snmp snmptrap spideroak-lansync squid ssh synergy syslog syslog-tls telnet tftp tftp-client tinc tor-socks transmission-client vdsm vnc-server wbem-https xmpp-bosh xmpp-client xmpp-local xmpp-server zabbix-agent zabbix-server
      ```

     Iit's important to know which serivces are running on your system. 
     
   -  The `--get-services` command option provides insight into which services are running. 
      
    - Based on your needs, disable the ones that are not critical to business operations. This is a form of system hardening.


5. Next, we will block all traffic coming from the Fifth Street location. The IP address associated with that location is `10.10.0.10`.

   Run the following command: 

   - `sudo firewall-cmd --zone=work --add-rich-rule='rule family="ipv4" source address="10.10.0.10" reject'`

        
        
        - `--add-rich-rule=`: The option to add a new rich rule.
        - `rule family="ipv4"`: Limits the rule to the IPV4 protocol.
        - `source address="10.10.0.10"`: The source IP address.
        - `reject`: The option to reject IPV4 addresses from the source address.


   Run: `sudo firewall-cmd --zone=work --list-all`

   Output should look similar to:

   ```bash
   work (active)
   target: default
   icmp-block-inversion: no
   interfaces: eth1
   sources: 
   services: ssh dhcpv6-client
   ports: 
   protocols: 
   masquerade: no
   forward-ports: 
   source-ports: 
   icmp-blocks: 
   rich rules: 
         rule family="ipv4" source address="10.10.0.10" reject
   ```

   - As seen in the output above, the new "rich rule" has been successfully added to our `work` zone and will be applied to the binded interface of `eth1`.

   - Note: The `reject` option in our rich rule will block all traffic from the `10.10.0.10` network.

   - Rules are erased when the firewall reboots. To save the configuration permanently to the database, add the `--permanent` option to the end of the rich rule.

6. We will also block ICMP pings from entering this network, in order to mitigate against DoS attacks. A ping request is also a technique that malicious actors use to acquire information about their targets.

    Run the following command:
   - `sudo firewall-cmd --zone=work --add-icmp-block=echo-reply --add-icmp-block=echo-request`


      - `--add-icmp-block`: Command option used to block ICMP protocols.

   Run `sudo firewall-cmd --zone=work --list-all`

   Output should look similar to:

    ```bash
    work (active)
    target: default
    icmp-block-inversion: no
    interfaces: eth1
    sources: 
    services: ssh dhcpv6-client
    ports: 
    protocols: 
    masquerade: no
    forward-ports: 
    source-ports: 
    icmp-blocks: echo-reply echo-request
    rich rules: 
            rule family="ipv4" source address="10.10.0.10" reject
    ```
    
- We can see that `icmp-blocks` will block `echo-reply` and `echo-request`.

### Commands Recap:

```bash
# Start the firewalld service
sudo /etc/init.d/firewalld start
```

```bash
# View all the firewall Zones
sudo firewall-cmd --list-all-zones
```

```bash
# Assign the interface eth1 to zone 'home'
sudo firewall-cmd --zone=home --change-interface=eth1
```

Firewalld also allows you to enable services per zone.

```bash
# List all available services to allow or deny
sudo firewall-cmd --get-services
```

```bash
# List all services applied to zone 'DMZ'
sudo firewall-cmd zone=DMZ --list-all
```
Setting rules with Firewalld
```bash
# Block traffic from address 10.10.10.10 in the office zone.
sudo firewall-cmd --zone=office --add-rich-rule="rule family='ipv4' source address='10.10.10.10' reject"
```
