---
title: VLAN configuration
parent: NetworkConfiguration
nav_order: 1
---
## VLAN Configuration

For devices on the same subnet you may need to use a bridge to
allow these devices to communicate. Bridging refers to the process of
combining multiple network interfaces into a single logical interface.
This new interface is called a bridge.

When creating a bridge there are a few flags users can add to set
the settings for:
**VLAN Aware Bridge Mode** and **VLAN Default Port VLAN ID**.

### VLAN Aware Bridge Mode for Filtering

Linux bridging supports VLAN Aware Bridge Mode, which allows a
bridge to handle VLAN IDs attached to frames crossing it,
including tagging and untagging frames and sending a frame 
belonging to a given VLAN only to ports configured to accept it.

VLAN tags are used to identify different VLANs in a network. 
When a device receives a frame, the VLAN tag can be used to determine 
to which VLAN the frame belongs.

Use the ``vlan_filtering`` flag on creation to toggle VLAN Aware Bridge Mode:
- The ``vlan_filtering 1`` flag activates the VLAN Aware Bridge Mode.
- The ``vlan_filtering 0`` flag turns off the VLAN Aware Bridge Mode.

### VLAN Default Port VLAN ID

A Default Port VLAN ID, also known as Default PVID, is used to
designate the VLAN to which untagged frames received on a bridge should be
associated to by default this will be VLAN1.

Use the ``vlan_default_pvid`` flag on creation to set the Default Port VLAN ID:
- The ``vlan_default_pvid S{Number}`` flag set the Default Port VLAN ID to VLAN ${Number}.
  - You may assign the default to any number of VLANs between 1 and 255.
- The ``vlan_filtering 0`` flag does not set any default PVID and
  does not configure VLANs on ports.

This setting can be changed.

### Creating a Bridge

Use the following command to create a bridge with the default settings:

``ip link add name ${Bridge Name} type bridge``

The following flags can be added to create a bridge with customized settings:

``ip link add name ${Bridge Name} type bridge vlan_filtering ${Number} vlan_default_pvid ${Number}``

After creating a bridge bring it up with the following commnad:

``ip link set ${Bridge Name} up``

Finally, add an IP address to the newly created bridge. 

``ip addr add ${insert IP address} dev ${Bridge Name}``

The bridge will now be listed alongside all other network interfaces. 
Run the command ``ip link show`` to list all interfaces.


### Establishing a VLAN

To establish a VLAN enslave the interfaces of connected devices to a bridge.

``bridge vlan add vid ${VLAN ID} dev ${Interface Name}``










