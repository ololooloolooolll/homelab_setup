# Setting up HA with keepalived

## Summary

keepalived is a linux service that uses VRRP-like protocol to assign a virtual IP to one of two peers.

Example: 

Router1 = 192.168.1.1
Router2 = 192.168.1.2
Virtual/shared ip = 192.168.1.254

With that configuration, you can setup you DNS server to use the 254 address and which ever is up 1 or 2 will reply...
Or setup a web page, or any service you need HA...


**Documentation: keepalived.readthedocs.io**

## Installation

On ubuntu and debian like systems, apt should find the required packages

    apt install keepalived
    
## Configuration

The configuration file for keepalived is /etc/keepalived/keepalived.conf

sample:

    vrrp_instance VI_1 {
      state [MASTER|BACKUP]
      interface ens192
      virtual_router_id 254
      priority [150|100]
      advert_int 1
      unicast_src_ip 192.168.1.[1|2]
      unicast_peer {
        192.168.1.[1|2]
      }
      authentication {
        auth_type PASS
        auth_pass 8charmax
      }
      virtual_ipaddress {
        192.168.1.254
      }
    }



## Linux services

Start means start now.
Enable means start at boot.
Status means printout status.

    systemctl start keepalived.service
    systemctl enable keepalived.service
    systemctl status keepalived.service 
    
