# vagrant-networking-demo
Simple Vagrant networking demo with Ubuntu and OpenBSD

This project demonstrates the use of Vagrant to deploy multiple VM's to test networking features such as routing. The idea is to provide the basic necessities to get a unix based networking lab running. There is a demo with Ubuntu as it allows for the quickest way to test Vagrant. There is also a demo with OpenBSD because that OS requires a couple of tricks to play nicely with Vagrant.

**Ubuntu**  
The project folder 3-nodes-ubuntu contains a Vagrant file that can be used to deploy three VM's in the following topology:  
client1 <-> router <-> client2  
  
For this project the standard Ubuntu image for Vagrant ubuntu/trusty64 was used. That's the name of the Ubuntu 14.04 LTS distribution. The machines are hooked up via private networks. Because Vagrant requires the presence of a NAT interface on each machine to do its stuff, a workaround is implemented. The Vagrant NAT interface is set to dhcp and that automatically overrides any default gateway you might set elsewhere on unix based OS'es. This Vagrant file contains commands to delete this default route on setup and optionally implement your own alternative. Furthermore, IPv4 forwarding is enabled on the router machine. 


**OpenBSD**  
box gepakt en aangepast als volgt:

