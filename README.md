## Part 1. **ipcalc** tool

##### Start a virtual machine (hereafter -- ws1)

#### 1.1. Networks and Masks

##### Define and write in the report:

##### 1) network address of _192.167.38.54/13_

##### 2) conversion of the mask _255.255.255.0_ to prefix and binary, _/15_ to normal and binary, _11111111.11111111.11111111.11110000_ to normal and prefix

##### 3) minimum and maximum host in _12.167.38.4_ network with masks: _/8_, _11111111.11111111.00000000.00000000_, _255.255.254.0_ and _/4_

#### 1.2. localhost

##### Define and write in the report whether an application running on localhost can be accessed with the following IPs: _194.34.23.100_, _127.0.0.2_, _127.1.0.1_, _128.0.0.1_

#### 1.3. Network ranges and segments

##### Define and write in a report:

##### 1) which of the listed IPs can be used as public and which only as private: _10.0.0.45_, _134.43.0.2_, _192.168.4.2_, _172.20.250.4_, _172.0.2.1_, _192.172.0.1_, _172.68.0.2_, _172.16.255.255_, _10.10.10.10_, _192.169.168.1_

##### 2) which of the listed gateway IP addresses are possible for _10.10.0.0/18_ network: _10.0.0.1_, _10.10.0.2_, _10.10.10.10_, _10.10.100.1_, _10.10.1.255_

## Part 2. Static routing between two machines

##### Start two virtual machines (hereafter -- ws1 and ws2)

##### View existing network interfaces with the `ip a` command

- Add a screenshot with the call and output of the used command to the report.

##### Describe the network interface corresponding to the internal network on both machines and set the following addresses and masks: ws1 — _192.168.100.10_, mask _/16 _, ws2 — _172.24.116.8_, mask _/12_

- Add screenshots of the changed _etc/netplan/00-installer-config.yaml_ file for each machine to the report.

##### Run the `netplan apply` command to restart the network service

- Add a screenshot with the call and output of the used command to the report.

#### 2.1. Adding a static route manually

##### Add a static route from one machine to another and back using a

`ip r add` command.

##### Ping the connection between the machines

- Add a screenshot with the call and output of the used commands to the report.

#### 2.2. Adding a static route with saving

##### Restart the machines

##### Add static route from one machine to another using _/etc/netplan/00-installer-config.yaml_ file

- Add screenshots of the changed _/etc/netplan/00-installer-config.yaml_
  file to the report.

##### Ping the connection between the machines

- Add a screenshot with the call and output of the used command to the report.

## Part 3. **iperf3** utility

- In this task you need to use ws1 and ws2 from _Part 2_.

#### 3.1. Connection speed

##### Convert and write results in the report: 8 Mbps to MB/s, 100 MB/s to Kbps, 1 Gbps to Mbps

#### 3.2. **iperf3** utility

##### Measure connection speed between ws1 and ws2

- Add a screenshots with the call and output of the used commands to the report.

## Part 4. Network firewall

- In this task you need to use ws1 and ws2 from _Part 2_.

#### 4.1. **iptables** utility

##### Create a _/etc/firewall.sh_ file simulating the firewall on ws1 and ws2:

```shell
#!/bin/sh

# Deleting all the rules in the "filter" table (default).
iptables -F
iptables -X
```

##### The following rules should be added to the file in a row:

##### 1) on ws1 apply a strategy where a deny rule is written at the beginning and an allow rule is written at the end (this applies to points 4 and 5);

##### 2) on ws2 apply a strategy where an allow rule is written at the beginning and a deny rule is written at the end (this applies to points 4 and 5);

##### 3) open access on machines for port 22 (ssh) and port 80 (http);

##### 4) reject _echo reply_ (machine must not ping, i.e. there must be a lock on OUTPUT);

##### 5) allow _echo reply_ (machine must be pinged);

- Add screenshots of the _/etc/firewall_ file for each machine to the report.

##### Run the files on both machines with `chmod +x /etc/firewall.sh` and `/etc/firewall.sh` commands.

- Add screenshots of both files running to the report;
- Describe in the report the difference between the strategies used in the first and second files.

#### 4.2. **nmap** utility

##### Use **ping** command to find a machine which is not pinged, then use **nmap** utility to show that the machine host is up

_Check: nmap output should say: `Host is up`_.

- Add screenshots with the call and output of the **ping** and **nmap** commands to the report.

## Part 5. Static network routing

Network: \
![part5_network](misc/images/part5_network.png)

##### Start five virtual machines (3 workstations (ws11, ws21, ws22) and 2 routers (r1, r2))

#### 5.1. Configuration of machine addresses

##### Set up the machine configurations in _etc/netplan/00-installer-config.yaml_ according to the network in the picture.

- Add screenshots of the _etc/netplan/00-installer-config.yaml_ file for each machine to the report.

##### Restart the network service. If there are no errors, check that the machine address is correct with the `ip -4 a`command. Also ping ws22 from ws21. Similarly ping r1 from ws11.

- Add screenshots with the call and output of the used commands to the report.

#### 5.2. Enabling IP forwarding.

##### To enable IP forwarding, run the following command on the routers:

`sysctl -w net.ipv4.ip_forward=1`.

_With this approach, the forwarding will not work after the system is rebooted._

- Add a screenshot with the call and output of the used command to the report.

##### Open _/etc/sysctl.conf_ file and add the following line:

`net.ipv4.ip_forward = 1`
_With this approach, IP forwarding is enabled permanently._

- Add a screenshot of the changed _/etc/sysctl.conf_ file to the report.

#### 5.3. Default route configuration

Here is an example of the `ip r' command output after adding a gateway:

```
default via 10.10.0.1 dev eth0
10.10.0.0/18 dev eth0 proto kernel scope link src 10.10.0.2
```

##### Configure the default route (gateway) for the workstations. To do this, add `default` before the router's IP in the configuration file

- Add a screenshot of the _etc/netplan/00-installer-config.yaml_ file to the report.

##### Call `ip r` and show that a route is added to the routing table

- Add a screenshot with the call and output of the used command to the report.

##### Ping r2 router from ws11 and show on r2 that the ping is reaching. To do this, use the `tcpdump -tn -i eth0`

command.

- Add screenshots with the call and output of the used commands to the report.

#### 5.4. Adding static routes

##### Add static routes to r1 and r2 in configuration file. Here is an example for r1 route to 10.20.0.0/26:

```shell
# Add description to the end of the eth1 network interface:
- to: 10.20.0.0/26
  via: 10.100.0.12
```

- Add screenshots of the changed _etc/netplan/00-installer-config.yaml_ file for each router to the report.

##### Call `ip r` and show route tables on both routers. Here is an example of the r1 table:

```
10.100.0.0/16 dev eth1 proto kernel scope link src 10.100.0.11
10.20.0.0/26 via 10.100.0.12 dev eth1
10.10.0.0/18 dev eth0 proto kernel scope link src 10.10.0.1
```

- Add a screenshot with the call and output of the used command to the report.

##### Run `ip r list 10.10.0.0/[netmask]` and `ip r list 0.0.0.0/0` commands on ws11.

- Add a screenshot with the call and the output of the used commands to the report.
- Explain in the report why a different route other than 0.0.0.0/0 had been selected for 10.10.0.0/\[netmask\] although it could be the default route.

#### 5.5. Making a router list

Here is an example of the **traceroute** utility output after adding a gateway:

```
1 10.10.0.1 0 ms 1 ms 0 ms
2 10.100.0.12 1 ms 0 ms 1 ms
3 10.20.0.10 12 ms 1 ms 3 ms
```

##### Run the `tcpdump -tnv -i eth0` dump command on r1

##### Use **traceroute** utility to list routers in the path from ws11 to ws21

- Add a screenshots with the call and the output of the used commands (tcpdump and traceroute) to the report;
- Based on the output of the dump on r1, explain in the report how path construction works using **traceroute**.

#### 5.6. Using **ICMP** protocol in routing

##### Run on r1 network traffic capture going through eth0 with the

`tcpdump -n -i eth0 icmp` command.

##### Ping a non-existent IP (e.g. _10.30.0.111_) from ws11 with the

`ping -c 1 10.30.0.111` command.

- Add a screenshot with the call and the output of the used commands to the report.

## Part 6. Dynamic IP configuration using **DHCP**

"Our next step is to learn more about **DHCP** service, which you already know."

**== Task ==**

_In this task you need to use virtual machines from Part 5._

##### For r2, configure the **DHCP** service in the _/etc/dhcp/dhcpd.conf_ file:

##### 1) Specify the default router address, DNS-server and internal network address. Here is an example of a file for r2:

```shell
subnet 10.100.0.0 netmask 255.255.0.0 {}

subnet 10.20.0.0 netmask 255.255.255.192
{
    range 10.20.0.2 10.20.0.50;
    option routers 10.20.0.1;
    option domain-name-servers 10.20.0.1;
}
```

##### 2) Write `nameserver 8.8.8.8` in a _resolv.conf_ file

- Add screenshots of the changed files to the report.

##### Restart the **DHCP** service with `systemctl restart isc-dhcp-server`. Reboot the ws21 machine with `reboot` and show with `ip a` that it has got an address. Also ping ws22 from ws21.

- Add a screenshot with the call and the output of the used commands to the report.

##### Specify MAC address at ws11 by adding to _etc/netplan/00-installer-config.yaml_:

`macaddress: 10:10:10:10:10:BA`, `dhcp4: true`

- Add a screenshot of the changed _etc/netplan/00-installer-config.yaml_ file to the report.

##### Сonfigure r1 the same way as r2, but make the assignment of addresses strictly linked to the MAC-address (ws11). Run the same tests

- Describe this part in the report the same way as for r2.

##### Request IP address update from ws21

- Add screenshots of IP before and after update to the report;
- Describe in the report what **DHCP** server options were used in this point.

## Part 7. **NAT**

"And finally, the cherry on the cake, let me tell you about network address translation mechanism."

**== Task ==**

_In this task you need to use virtual machines from Part 5_

##### In _/etc/apache2/ports.conf_ file change the line `Listen 80` to `Listen 0.0.0.0:80`on ws22 and r1, i.e. make the Apache2 server public

- Add a screenshot of the changed file to the report

##### Start the Apache web server with `service apache2 start` command on ws22 and r1

- Add screenshots with the call and the output of the used command to the report.

##### Add the following rules to the firewall, created similarly to the firewall from Part 4, on r2:

##### 1) delete rules in the filter table — `iptables -F`

##### 2) delete rules in the "NAT" table — `iptables -F -t nat`

##### 3) drop all routed packets — `iptables --policy FORWARD DROP`

##### Run the file as in Part 4

##### Check the connection between ws22 and r1 with the `ping` command

_When running the file with these rules, ws22 should not ping from r1_

- Add screenshots with the call and the output of the used command to the report.

##### Add another rule to the file:

##### 4) allow routing of all **ICMP** protocol packets

##### Run the file as in Part 4

##### Check connection between ws22 and r1 with the `ping` command

_When running the file with these rules, ws22 should ping from r1_

- Add screenshots with the call and the output of the used command to the report.

##### Add two more rules to the file:

##### 5) enable **SNAT**, which is masquerade all local IP from the local network behind r2 (as defined in Part 5 — network 10.20.0.0)

_Tip: it is worth thinking about routing internal packets as well as external packets with an established connection_

##### 6) enable **DNAT** on port 8080 of r2 machine and add external network access to the Apache web server running on ws22

\*Tip: be aware that when you will try to connect, there will be a new tcp connection for ws22 and port 80

- Add a screenshot of the changed file to the report

##### Run the file as in Part 4

_Before testing it is recommended to disable the **NAT** network interface in VirtualBox (its presence can be checked with `ip a` command), if it is enabled_

##### Check the TCP connection for **SNAT** by connecting from ws22 to the Apache server on r1 with the `telnet [address] [port]` command

##### Check the TCP connection for **DNAT** by connecting from r1 to the Apache server on ws22 with the `telnet` command (address r2 and port 8080)

- Add screenshots with the call and the output of the used commands to the report.

## Part 8. Bonus. Introduction to **SSH Tunnels**

_In this task you need to use virtual machines from Part 5._

##### Run a firewall on r2 with the rules from Part 7

##### Start the **Apapche** web server on ws22 on localhost only (i.e. in _/etc/apache2/ports.conf_ file change the line `Listen 80` to `Listen localhost:80`)

##### Use _Local TCP forwarding_ from ws21 to ws22 to access the web server on ws22 from ws21

##### Use _Remote TCP forwarding_ from ws11 to ws22 to access the web server on ws22 from ws11

##### To check if the connection worked in both of the previous steps, go to a second terminal (e.g. with the Alt + F2) and run the `telnet 127.0.0.1 [local port]` command.

- In the report, describe the commands that you need for doing these 4 steps and add screenshots of their call and output.
