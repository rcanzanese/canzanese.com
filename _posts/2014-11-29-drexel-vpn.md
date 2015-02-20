---
layout: post
title: Connecting to Drexel's VPN Server using Linux
---

*These instructions assume you are using Network-Manager and running Ubuntu 14.04 or OpenSuse 13.1.  I have not tested this on other versions.*

First check your prerequisites.  Drexel uses a Cisco VPN server.  Make sure you have network-manager-vpnc installed in Ubuntu:

    sudo apt-get install network-manager-vpnc

Or in OpenSuse:

    sudo zypper in NetworkManager-vpnc

Next, use Network Manager to to setup the VPN connection.


* Right click on the Network Manager tray icon and select VPN Connections -> Configure VPN
* Select New.
* For connection type, select "Cisco Compatible VPN (vpnc)".
*  Enter the following information (from Drexel's VPN instructions)
   * Connection name: Drexel VPN (just so you don't get confused later)
   * Gateway: vpn01.irt.drexel.edu
   * User name:  your Drexel username
   * Password: your Drexel password
   * Group ID: DrexelVPN
   * Group Password: n0cB5tqdmV (the second character is a zero) 
*    Save the new connection when finished.
*    Connect to the VPN by right clicking the Network Manager tray icon and selecting VPN Connections -> Drexel VPN


