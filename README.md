# Abstract

Docker for L2TP over IPSec VPN server
User Authentication: Password

This server is based SoftEther VPN.

  - http://www.softether.org/
  - http://ja.softether.org/

The following options are enabled.
  * SecureNatEnable
  * NatEnable
  * DHCPEnable

# Usage

```shell
$ docker-compose up -d
```

## Options

The following arguments can be changed in docker-compose.yml.

|Argument         |Default value  |
|:-               |:-             |
|SOFTETHER_VERSION|v4.22-9634-beta|
|HUB_NAME         |MyHub          |
|HUB_PASSWD       |hogehoge       |
|USER_NAME        |MyVpnUser      |
|USER_PASSWD      |fugafuga       |
|COMMON_SECRET    |hogefuga       |

## Network settings

Transport UDP Ports: UDP 500 and 4500

Allow both ports on the firewall. Add UDP port forwarding for both 500 & 4500 on the NAT.
