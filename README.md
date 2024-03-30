Hans - IP over ICMP
===================

Hans makes it possible to tunnel IPv4 through ICMP echo packets, so you could call it a ping tunnel. This can be useful when you find yourself in the situation that your Internet access is firewalled, but pings are allowed.

Hans – IP over ICMP
Hans makes it possible to tunnel IPv4 through ICMP echo packets, so you could call it a ping tunnel. This can be useful when you find yourself in the situation that your Internet access is firewalled, but pings are allowed.

Hans runs on Linux as a client and a server. It runs on Mac OS X, iPhone/iPod touch, FreeBSD, OpenBSD and Windows as a client only.

Is is inspired by icmptx and adds some features:

Features
Reliability: Hans works reliably in situations when the client is behind a firewall that allows only one echo reply per request.
Security: Hans uses a challenge-response based login mechanism.
Multiple clients: Hans currently supports up to 253 clients, which is the number of available IPs on the virtual subnet.
Easy setup: Hans automatically assigns IP addresses.
For the iPhone/iPod touch version have a look at tunemu.

Get Hans
Hans source. Hans Mac OS X binary. Hans Windows binary.

Browse the source and contribute on Github.

View the changelog.

Use Hans
First, make sure you kernel supports tun devices. For Mac OS X you can get the drivers here. On Windows you have to install a tap device driver by downloading the Windows Installer of OpenVPN and selecting "TAP Virtual Ethernet Adapter" during the installation.

To compile hans, unpack it and run "make":  

tar -xzf hans-version.tar.gz  
cd hans-version  
make  
To run as a server (as root):  
./hans -s 10.1.2.0 -p password  
This will create a new tun device and assign the IP 10.1.2.1 to it. Note that Hans can not receive echo requests on BSD systems. Therefore the server only works on Linux.

To run as a client (as root):  
./hans -c server_address -p password  
This will connect to the server at "server_addess", create a new tun device and assign an IP from the network 10.1.2.0/24 to it.

Now you can run a proxy on the server or let it act as a router and use NAT to allow the clients to access the Internet.

On Windows you must run your command prompt as Administrator in order for hans to work.

Troubleshoot / Tweak
If you are behind a firewall that filters icmp packets in any way, which is likely, you might have to make some adjustments. During this process it is useful to add the "-fv" options to the command. With this hans stays attached to the terminal and shows some debug output.

First, you should tell your operating system not to respond to echo requests. On Linux this can be done by:  

echo 1 >    /proc/sys/net/ipv4/icmp_echo_ignore_all  
Now you might want to add the "-r" option to the server command. This tells Hans also to respond to ordinary pings.

By default the client is configured to send 10 poll "echo requests" that can be answered by the server, when data needs to be transmitted. You might want to lower this value using the "-w" flag, if you experience packet loss. You can also try to raise this value to increase the throughput of the tunnel.

In some cases it might be necessary to tell the client to change the echo id or sequence number with each request. This might have a serious impact on performance. You should first try the "-q" flag and if this does not work, the "-i" flag.

Finally you can tell Hans to run as a different user via the "-u" flag.

Note that when you run Hans without any parameters you get a short description of the available commands.

Please report at the issue tracker if there are further problems.

http://code.gerade.org/hans/
