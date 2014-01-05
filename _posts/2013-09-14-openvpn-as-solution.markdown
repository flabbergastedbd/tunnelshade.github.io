---
layout: post
title: "OpenVPN - My One Ultimate solution for networking woes"
categories: Hacks
tags: tricks linux
image: http://tkjune.com/uploads/2012/11/openvpntech_logo1.png
---

Let me start by explaining the crap I deal with :-

- In my university, we have to use a proxy server (squid proxy) which has only port 80 open. Yes it is true :'(
- Some sites like Youtube, Facebook etc.. are blocked.  

The above two problems gave me hell lot of trouble. I have to use proxy settings everywhere (pacman, git etc..) :(.
In some cases a simple HTTP  Proxy will not work everywhere (like IRC Clients etc..).

Some short term solutions I used before the real stuff :- 

- Used [proxychains](http://proxychains.sourceforge.net/) & openssh to create a local socks proxy which I could use in places where it is supported.
- Used [VPNBook](http://www.vpnbook.com/) for some casual stuff.

The first solution did not solve my proxy settings issue, while the second solution is really not secure :P.

Finally this is what I did :-

In single line = Arch Linux VPS + OpenVPN  = WIN : )

For non-geeks, here goes the full stuff :-

- Buy a VPS with any good linux distribution. I prefer [Tilaa](https://www.tilaa.com/) , because of their Cheap **Unlimited Bandwidth** plan + Arch Linux distro template.
- Install OpenVPN on VPS :P - Generate required certs & keys for the server & one client ( [Help here](https://wiki.archlinux.org/index.php/OpenVPN#Create_a_Public_Key_Infrastructure_.28PKI.29_from_scratch) )
- Edit the configuration files for both server & client. ( [Again help here](https://wiki.archlinux.org/index.php/OpenVPN#A_basic_L3_IP_routing_configuration) )
- Start the openvpn server & try to connect from the client as suggested in the arch wiki.

**NOTE:** If you are behind a **HTTP Proxy** with **basic or ntlm** auth, then you can follow the steps [here](http://openvpn.net/index.php/open-source/documentation/howto.html#http))

- If everything goes fine, now you must be able to connect to your openvpn server through http proxy.

Now comes the real part, with the help of openvpn we have the facility to redirect all network traffic through VPN. So if we could somehow make  the server redirect this traffic towards internet, there will be no  need to use proxy settings with all the applications in the client :). 

For non-geeks:-

- Follow [this guide](http://openvpn.net/index.php/open-source/documentation/howto.html#redirect) for adding directives in configuration files to route all client traffic through VPN.
- Finally, on the Arch VPS enable redirection of traffic with the help of [this guide](https://wiki.archlinux.org/index.php/OpenVPN#L2_Ethernet_bridging). ( Be careful with iptables as **MASQUERADE** doesn't work with OpenVZ vps )
- Now you must be able to connect to internet through your VPN, when your vpn is active.  

So there is no need to define proxy settings anywhere else because all the traffic is routed through VPN.

Happy ending to the story :-

- No need for any proxy settings anywhere else except for vpn client conf.
- All the data is encrypted so that eavesdropping & filtering is prevented.
- No need to use Tor Anonymity network, whose exit nodes are banned in many places.
