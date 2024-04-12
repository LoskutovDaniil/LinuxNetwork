## Part 1. ipcalc tool
1. Networks and MAsks
- 1) network address of 192.167.38.54/13
![](../materials/1.1.1.png)
- 2) conversion of the mask 255.255.255.0 to prefix and binary, /15 to normal and binary, 11111111.11111111.11111111.11110000 to normal and prefix
![](../materials/1.1.2.png)
- 3) minimum and maximum host in 12.167.38.4 network with masks: /8, 11111111.11111111.00000000.00000000, 255.255.254.0 and /4
![](../materials/1.1.3.png)
![](../materials/1.1.3.2.png)
2. localhost
- IPs 127.0.0.2, 127.1.0.1 can be accessed on localhost
![](../materials/1.2.1.png)
![](../materials/1.2.2.png)
3. Network ranges and segments
- 1) private: 10.0.0.45, 192.168.4.2, 172.20.250.4,  172.16.255.255, 10.10.10.10
![](../materials/1.3.1.png)
![](../materials/1.3.3.png)
- public 134.43.0.2, 172.0.2.1, 172.68.0.2, 192.172.0.1, 192.169.168.1
![](../materials/1.3.2.png)
![](../materials/1.3.4.png)
- 2) possible gateway IPs for 10.10.0.0/18 network: 10.10.0.2, 10.10.10.10
![](../materials/1.3.5.png)
## Part 2. Static routing between two machines
- 'ip a' on both machines 
![](../materials/2.png)
- netplan changed
![](../materials/2.0.1.png)
- "sudo netplan apply" and "ip a"
![](../materials/2.0.2.png)
1. Adding a static route manually and ping
![](../materials/2.1.png)
2. Adding a static route with saving
![](../materials/220.png)
- "sudo netplan apply" and "ping"
![](../materials/221.png)
## Part 3. iperf3 utility
1. Connection speed
8 Mbps = 1 MB/s,
100 MB/s = 800 000 Kbps,
1 Gbps = 1000 Mbps
2. iperf3 utility
![](../materials/3.png)
## Part 4. Network firewall
1. iptables utility
- Delete rules
![](../materials/40.png)
- update with rules firewall.sh
![](../materials/41.png)
- firewall applied, existense of rules checked, ping executed
![](../materials/410.png)
- Ping in the second file (for input of ws1) unsuccessful as first rules(which was DROP in ws1) have higher priority than the following rules.
2. nmap utility
- ping ws1 doesn't receive ring request
![](../materials/420.png)
![](../materials/421.png)
- nmap is used, hosts are up for both machines
![](../materials/42.png)
- saving dump
![](../materials/423.png)
## Part 5. Network firewall
1. Configuration of machine addresses
- etc/netplan/00-installer-config.yaml
![](../materials/510.png)
- "sudo netplan apply" then "ip -4 a" for machines
![](../materials/511.png)
- for routers
![](../materials/512.png)
- ping ws22 from ws21
![](../materials/513.png)
- ping r1 from ws11
![](../materials/514.png)
2. Enabling IP forwarding.
![](../materials/515.png)
- changing sysctl.conf file
![](../materials/521.png)
3. Default route configuration
- Configure the default route (gateway) for the workstations
![](../materials/530.png)
- ip r
![](../materials/531.png)
- ping request on ws11
![](../materials/532.png)
- tcpdump -tn -i eth1
![](../materials/533.png)
4. Adding static routes
- dd static routes to r1 and r2 in configuration file
![](../materials/540.png)
- ip r
![](../materials/541.png)
- ip r list on ws11
![](../materials/542.png)
- Different route other than 0.0.0.0/0 had been selected for 10.10.0.0/[netmask] because more precise (longer mask) route were specified int hte netplan.
5. Making a router list
- tcpdump -tnv -i enp0s8
![](../materials/544.png)
- traceroute on ws11
![](../materials/543.png)
- Passing through nodes and receiving pong reply traceroute shows them forming the list (route) of network.
6. Using ICMP protocol in routing
- Run tcpdump on r1 and ping a non-existent IP 10.30.0.111
![](../materials/56.png)
## Part 6. Dynamic IP configuration using DHCP
1. For r2, configure the DHCP service in the /etc/dhcp/dhcpd.conf file:
![](../materials/61.png)
2. write nameserver 8.8.8.8. in a resolv.conf file
![](../materials/620.png)
- Restart the DHCP service with systemctl restart isc-dhcp-server
![](../materials/621.png)
- change configuration to dynamic for ws21
![](../materials/622.png)
- sudo net plan apply, reboot, ip a
![](../materials/623.png)
- ping ws22 from ws21
![](../materials/624.png)
- Specify MAC address at ws11 by adding to etc/netplan/00-installer-config.yaml
![](../materials/625.png)
- for r1:
configure the DHCP service in the /etc/dhcp/dhcpd.conf file:
![](../materials/626.png)
3. write nameserver 8.8.8.8. in a resolv.conf file
![](../materials/627.png)
- Restart the DHCP service with systemctl restart isc-dhcp-server
![](../materials/628.png)
- ip a before update
![](../materials/629.png)
- ip a after update
![](../materials/630.png)
option -r were used. The -r flag explicitly releases the current lease, and once the lease has been released, the client exits.
## Part 7. NAT
- sudo nano /etc/apache2/ports.conf
![](../materials/70.png)
- start apache2
![](../materials/701.png)
- add rules to firewall
![](../materials/702.png)
- apply rules
![](../materials/703.png)
- ping r1 from ws22
![](../materials/704.png)
- allow forwarding
![](../materials/7005.png)
- ping succsessfull
![](../materials/706.png)
- enable SNAT
![](../materials/705.png)
- enable DNAT on port 8080
![](../materials/707.png)
- Check the TCP connection for SNAT by connecting from ws22 to the Apache server on r1 
![](../materials/708.png)
- Check the TCP connection for DNAT by connecting from r1 to the Apache server on ws22
![](../materials/709.png)
## Part 8. Bonus. Introduction to SSH Tunnels
- r2 furewall config is below, then chmod +x /etc/firewall.sh and /etc/firewall.sh
![](../materials/80.png)
- /etc/apache2/ports.conf file change on ws22
![](../materials/81.png)
- start apache2 on ws22
![](../materials/82.png)
- sudo ssh -L 9999:localhost:80 ws3@10.20.0.20
![](../materials/83.png)
- sudo ssh -R 7777:localhost:80 ws22@10.20.0.20
![](../materials/84.png)
- successfull entry
![](../materials/85.png)
- check ws21-ws22
![](../materials/86.png)
- check ws11-ws22
![](../materials/87.png)
- All snapshots during the project were taken
![](../materials/9.png)