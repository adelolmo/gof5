gof5 1 "October 29, 2021" gof5 "User Manual"
========================================

# NAME
gof5 - Open Source F5 BIG-IP VPN client

# DESCRIPTION
Open Source F5 BIG-IP VPN client.

# OPTIONS
The options are as follows:

**--server=URL**
    Server Url.

**--username=STRING**
    Username.

**--password=STRING**
    Password.

**--session=ID**
    Reuse a session ID.

**--ca-cert=FILE**
    Path to a custom CA certificate.

**--cert=FILE**
    Path to a user TLS certificate.

**--key=FILE**
    Path to a user TLS key.

**--close-session**
    Close HTTPS VPN session on exit.

**--debug**
    Show debug logs.

**--select**
    Select a server from available F5 servers.

**--profile-index=N**
    If multiple VPN profiles are found chose profile n.
    
**--version**
    Show version and exit cleanly.

# NOTES

When username and password are not provided, they will be asked if **$HOME/.gof5/cookies.yaml** 
file doesn't contain previously saved HTTPS session cookies or when the saved session is expired or 
explicitly terminated (**--close-session**).

Use **--close-session** flag to terminate an HTTPS VPN session on exit. Next startup will require 
a valid username/password.

Use **--select** to choose a VPN server from the list, known to a current server.

Use **--profile-index** to define a custom F5 VPN profile index. 

## CA certificate and TLS keypair

Use options below to specify custom TLS parameters:

**--ca-cert** - path to a custom CA certificate

**--cert** - path to a user TLS certificate

**--key** - path to a user TLS key

# CONFIGURATION

You can define an extra **$HOME/.gof5/config.yaml** file with contents:
```
# DNS proxy listen address, defaults to 127.0.0.245
# In BSD defaults to 127.0.0.1
# listenDNS: 127.0.0.1
# rewrite /etc/resolv.conf instead of renaming
# Linux only, required in cases when /etc/resolv.conf cannot be renamed
rewriteResolv: false
# experimental DTLSv1.2 support
# F5 BIG-IP server should have enabled DTLSv1.2 support
dtls: false
# TLS certificate check
insecureTLS: false
# Enable IPv6
ipv6: false
# driver specifies which tunnel driver to use.
# supported values are: wireguard or pppd.
# wireguard is default.
# pppd requires a pppd or ppp (in FreeBSD) binary
driver: wireguard
# When pppd driver is used, you can specify a list of extra pppd arguments
PPPdArgs: []
# disableDNS allows to completely disable DNS handling,
# i.e. don't alter system DNS (e.g. /etc/resolv.conf) at all
disableDNS: false
# TLS renegotiation support as defined in tls.RenegotiationSupport, disabled by default
renegotiation: RenegotiateNever
# A list of DNS zones to be resolved by VPN DNS servers
# When empty, every DNS query will be resolved by VPN DNS servers
dns:
- .corp.int.
  - .corp.
# for reverse DNS lookup
  - .in-addr.arpa.
# override DNS servers, provided by a VPN server profile
overrideDNS:
- 8.8.8.8
# override DNS search suffix, provided by a VPN server profile
overrideDNSSuffix:
- my.corp
# A list of subnets to be routed via VPN
# When not set, the routes pushed from F5 will be used
# Use "routes: []", if you don't want gof5 to manage routes at all
routes:
- 1.2.3.4
  - 1.2.3.5/32
```

# EXAMPLES

## Username and password
To connect to the VPN providing username and password.
    **sudo gof5 --server https://vpn-server.com --username USERNAME --password PASSWORD**

## Session ID
Alternatively you can use a session ID obtained during the web browser authentication 
(in case, when you have MFA). You can find the session ID by going to the VPN host in 
a web browser, logging in, and running this JavaScript in Developer Tools:
    **document.cookie.match(/MRHSession=(.*?); /)[1]**

To connect to the VPN reusing the SESSION_ID in case MFA is enabled.
    **sudo gof5 --server https://vpn-server.com --session SESSION_ID**

# AUTHOR
gof5 was written by kayrus <https://github.com/kayrus>

This manual page was written by Andoni del Olmo <andoni.delolmo@gmail.com>