# Setup Dnsmasq (Enable domain and subdomain with wildcard) on Ubuntu 16.04 or Higher

Dnsmasq is a lightweight, easy to configure DNS forwarder, designed to provide DNS (and optionally DHCP and TFTP) services to a small-scale network. It can serve the names of local machines which are not in the global DNS.

## Getting started

- Install dnsmasq
```console
$ sudo apt install dnsmasq
```

- Edit the file `/etc/NetworkManager/NetworkManager.conf`, and add the line `dns=dnsmasq` to the `[main]` section

```console
[main]
plugins=ifupdown,keyfile
dns=dnsmasq

[ifupdown]
managed=false

[device]
wifi.scan-rand-mac-address=no
```

- Modify `/etc/dnsmasq.conf` port=53 into port=5353

- Let NetworkManager manage `/etc/resolv.conf`
```console
$ sudo rm /etc/resolv.conf && sudo ln -s /var/run/NetworkManager/resolv.conf /etc/resolv.conf
```

- Create file `/etc/NetworkManager/dnsmasq.d/AppDevelopmentWildcards.conf`
```console
$ /etc/NetworkManager/dnsmasq.d/AppDevelopmentWildcards.conf
```

```console
address=/localhost.com/127.0.0.1 # Domain
address=/sub.localhost.com/127.0.0.1 # Subdomain
address=/.localhost.com/127.0.0.1 # Subdomain wildcard
```

- Restart dnsmasq service
```console
$ sudo service dnsmasq restart
```

- Restart network-manager
```console
$ sudo service network-manager restart
```

- Restore `/etc/resolv.conf` into default
```console
$ sudo rm /etc/resolv.conf && sudo ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```