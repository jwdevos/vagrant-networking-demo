# vagrant-networking-demo  
This project demonstrates the use of Vagrant to deploy multiple VM's to test networking features such as routing. The idea is to provide the basic necessities to get a unix based networking lab running. There is a demo with Ubuntu as it allows for the quickest way to test Vagrant. There is also a demo with OpenBSD because that OS requires a couple of tricks to play nicely with Vagrant.  
  
**Ubuntu**  
The project folder 3-nodes-ubuntu contains a Vagrant file that can be used to deploy three VM's in the following topology:  
client1 <-> router <-> client2  
  
For this project the standard Ubuntu image for Vagrant ubuntu/trusty64 was used. That's the name of the Ubuntu 14.04 LTS distribution. The machines are hooked up via private networks. Because Vagrant requires the presence of a NAT interface on each machine to do its stuff, a workaround is implemented. The Vagrant NAT interface is set to dhcp and that automatically overrides any default gateway you might set elsewhere on unix based OS'es. This Vagrant file contains commands to delete this default route on setup and optionally implement your own alternative. Furthermore, IPv4 forwarding is enabled on the router machine.  
  
When starting this project, you end up with three machines, the clients being able to reach each other through the router. This base can be extended to test things like setting up a web server on a client, then enabling iptables on the router and testing reachability to the server from the other client. That's just one example of the endless possibilities.  
  
**OpenBSD**  
The project folder 3-nodes-openbsd contains a Vagrant file that can be used to deploy three VM's in the following topology:  
client1 <-> fw <-> client2  
  
For this project the "generic/openbsd6" box was used, which is readily avaible from the Vagrant Cloud library. This box was chosen as it has timely updates and support for both VirtualBox and libvirt. 
  
When I first tried to use this box I ran into a problem. Vagrant sends SSH commands to the machines as part of the setup procedure. There was a permissions problem which I tracked down to broken sudo/doas configuration in the Vagrant box. To fix it, I first changed the following settings and then used Vagrant to export the machine to become a new box, with the settings fixed.  
'''
echo "#includedir /etc/sudoers.d/" > /etc/sudoers
touch /etc/doas.conf
echo "permit :wheel" >> /etc/doas.conf
echo "permit nopass keepenv root" >> /etc/doas.conf
echo "permit nopass keepenv vagrant" >> /etc/doas.conf
'''  
  
After fixing the settings, you just run the following commands from the same folder:  
'''
vagrant package --output obsd.box
vagrant box add obsd obsd.box
'''  
  
This gives you a new usable OpenBSD box in Vagrant, which you can use like this:  
'''  
config.vm.box = "obsd"
'''  
  
Like with the Ubuntu demo, there's some magic to hijack the default route where it's necessary, because of the required Vagrant NAT interface. Also, IPv4 forwarding is enabled on the firewall. The Vagrant synced folders don't work well with BSD distro's so that stuff gets disabled explicitly to avoid further hassles.  
  
The clients can reach each other through the firewall after starting the machines. This demo can be extended to test a firewall setup with pf for example. You can also add more nodes and test site to site tunnels with OpenVPN.  
