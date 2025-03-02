Wake on LAN (WoL) is a networking standard that allows a computer to be turned on or awakened from sleep mode by a network message. Here's a comprehensive explanation:

## How it works:
1. A special network packet called a "magic packet" is sent to the target computer(by broadcasting)
2. The magic packet contains the MAC address of the target computer repeated 16 times
3. The computer's network interface card (NIC) is always powered on in a low-power state, listening for this magic packet
4. When detected, the NIC signals the computer's motherboard to power on

## Requirements:
- The target computer must have a WoL-capable network interface card
- WoL must be enabled in the computer's BIOS/UEFI settings
- The network card must remain powered even when the computer is off
- In most cases, the computer needs to be connected via Ethernet (though some modern systems support WoL over Wi-Fi)

## Common uses:
- Remote administration of computers
- Starting up computers for scheduled maintenance
- Powering on work computers from home
- Server management in data centers

## The magic packet structure:
- 6 bytes of FF (0xFF FF FF FF FF FF)
- The target computer's MAC address repeated 16 times
- Usually sent as a UDP broadcast packet on port 7 or 9

## To use WoL:
1. Configure BIOS/UEFI settings to enable WoL
2. Note down the target computer's MAC address
3. Use WoL software or command-line tools to send the magic packet
4. The computer should power on within a few seconds

## Common limitations:
- Doesn't work across different subnets without special configuration: Packet Forwarding etc.
- May not work if the computer was forcefully shut down or lost power
- Some routers might block WoL packets by default
- Generally more reliable over wired connections than wireless

