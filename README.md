# Network-practices


# Basic LAN setup with DHCP(packet tracer)

# Objective 🎯

Basic LAN setup with DHCP. To stimulate the office network with three wired computers, one printer, and a wireless laptop. All connected to the same LAN. The router will provide DHCP to all devices, and we will test connectivity between them. 

# **System requirements**

- CISCO Packet Tracer

# Lan Setup

<img src="https://imgur.com/a/RczkCDz" height="80%" width="80%"/>

**Installation and Setup** 

***Step 1 : Open Packet Tracer***

1. Add all end devices to the workplace (PC1, PC2, PC3, Printer0, Laptop0)
2. From the network section, add 1 Switch, 1 Router, and 1 Wireless Router. 

***Step 2: Physically Connect Devices*** 

    **Wired Connections**

1. Click on the cable icon and select a copper straight-through cable 

  2.  Connect each PC to the switch using the copper straight-through cable

  - PC-0 to any port on Switch-0 (e.g. FastEthernet 0/1).

   -PC-2 to FastEthernet 0/2.

   -PC-3 to FastEthernet 0/3.

1. Connect the Printer to the Switch (e.g. FastEthernet 0/4)
2. Connect the Router to the Switch using a straight-through cable( Router’s GigabitEthernet 0/0 to Switch’s FastEthernet 0/5)

  note: straight through cable = connecting unlike devices

    **Wireless Connections**

1. Connect WirelessRouter-0 to the Router.

          -Use a  crossover cable to connect WirelessRouter-0’s internet port to Router’s GigabitEthernet 0/1.

  6.  Configure the Router 

        [1.Click](http://1.Click) on Router-0, go to the CLI tab

        2.Configure the Router’s GigabitEthernet 0/0 interface

Router>en
Router#conf t
Router(config)#
Router(config)#int gi0/0                                                                           Router(config-if)#ip address 192.168.1.1 255.255.255.0                                 Router(config-if)# no shutdown
Router(config-if)# exit

       3. Configure The Router’s GigabitEthernet 0/1 interface (for WirelessRouter-0)

Router(config)#int gi0/1
Router(config-if)#ip address 192.168.2.1 255.255.255.0                          Router(config-if)# no shutdown
Router(config-if)#exit

Router(config)#do wr
Building configuration...
[OK]
Router(config)#int gi0/0
Router(config-if)#no shut

      4. Set up a DHCP Pool for wired devices on the 192.168.1.0/24 network:

Router(config)# ip dhcp pool LAN
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# exit

      5. Set up a DHCP Pool for wireless devices on the 192.168.2.0/24 network:
Router(config)# ip dhcp pool WLAN
Router(dhcp-config)# network 192.168.2.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.2.1
Router(dhcp-config)# exit

       6. Save the configuration:
Router# write memory

***Step 4: Configure the Wireless Router***

1. Click on WirelessRouter-0, go to the GUI tab.
2. Set the Internet IP Address:
-Select DHCP for the Internet Connection Type (this allows the Wireless
Router to obtain an IP from Router-0).
3. Go to the Wireless section:
-Change the SSID to something like "SmallOfficeWiFi".
-Set WPA2-PSK security and create a passphrase (e.g., "office123").
4. Go to the LAN settings and configure:
- IP Address: 192.168.2.1
- Subnet Mask: 255.255.255.0
5. Enable the DHCP server to provide IP addresses on the 192.168.2.0/24 network.

***Step 5: Configure the End Devices***
**For Wired PCs**

1. Click on PC-0, then go to the Desktop tab and open the IP Configuration window.
- Set the IP configuration to DHCP (this allows the PC to receive an IP
address from the router).
2. Repeat this step for PC-1, PC-2, and Printer-0.

**For Wireless Laptop**

1. Click on Laptop-0, then go to the Desktop tab and open the IP Configuration
window.
- Select DHCP for automatic IP addressing.
2. Go to the Laptop-0's Config tab, select Wireless0, and choose the
"SmallOfficeWiFi" SSID.
- Enter the passphrase ("office123") and connect

 **For Printer**
**Step 1: Choose a Static IP Address**
—The printer should have an IP in the same network as the other wired devices but outside the DHCP pool to avoid conflicts.
—For example, if your DHCP pool is from 192.168.1.2 to 192.168.1.100, you can
assign the printer an IP like 192.168.1.101.

**Step 2: Set the Printer to Use a Static IP**
In the IP Configuration window, uncheck the box that says DHCP to disable
dynamic IP assignment.
Manually enter the following details:
● IP Address: Set this to the chosen static IP, for example, 192.168.1.101.
● Subnet Mask: This will typically be 255.255.255.0, assuming you're using a
standard class C network.
● Default Gateway: Set this to the IP of the router, which is 192.168.1.1 (the router
that serves as the default gateway for the wired LAN).
● DNS Server: Enter the same IP as the default gateway (192.168.1.1) or a public
DNS server (like 8.8.8.8).
**Click Apply to save the configuration.**

**Test the Setup**

1. From PC-0, open the Command Prompt and type:
Ipconfig
Verify the assigned IP address (should be in the range of 192.168.1.x).
Ping the router (ping 192.168.1.1) to check the connection.
2. From Laptop-0, open the Command Prompt, type:
Ipconfig
Ensure the IP is in the 192.168.2.x range.
Ping the router (ping 192.168.2.1) to check the wireless connection.
3. Test Connectivity to the Printer
Click on any of the PCs (e.g., PC-0), go to the Desktop tab, and open the
Command Prompt.
Type the following command to ping the printer’s static IP address
ping 192.168.1.101
—If everything is configured correctly, you should receive reply messages
confirming that the PC can communicate with the printer.
