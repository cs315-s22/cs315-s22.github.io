---
layout: assignment
due: 2022-02-09 14:00:00 -0800
permalink: assignments/project01-port-forwarding.html
---
### Requirements
1. If you want to leave your pi at home, you must be able to control the WiFi router at home 

### Your Home Network
1. Your home router has an IPv4 address on the public Internet. If you use a website like [https://whatismyipaddress.com/](https://whatismyipaddress.com/) from your home network, you can see an address like `73.240.154.193` (just an example)
1. Your Pi has a private network address on your home network. You can see it like this:
    ```sh
    pi@pi3:~ $ ifconfig wlan0
    wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 192.168.176.182  netmask 255.255.255.0  broadcast 192.168.176.255
            inet6 fd2c:615:9bdd:4066:c8fc:a67b:304d:4b3f  prefixlen 64  scopeid 0x0<global>
            inet6 fe80::fac8:5dab:1e41:85bd  prefixlen 64  scopeid 0x20<link>
            ether b8:27:eb:49:ce:77  txqueuelen 1000  (Ethernet)
            RX packets 9854  bytes 1228582 (1.1 MiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 764  bytes 111343 (108.7 KiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    ```
    In this example, my Pi has the private network address `192.168.176.182`
1. Your home router probably allows "port forwarding", where network traffic on a given network port can be forwarded to a private address inside the router's network. `ssh` uses port 22 by default.
1. You want to configure your router so that port 22 traffic to the router's public address gets forwarded to port 22 on your Pi. Every router has its own UI, but the settings should be called Port Forwarding.
1. Once you get that set up, you should be able to take your laptop to USF Wireless or CSLabs WiFi and ssh to your Pi at home
    ```sh
    $ ssh pi@73.240.154.193
    ```

### Dynamic DNS (optional)

1. There are free dynamic DNS services like [https://freeddns.dynu.com/](https://freeddns.dynu.com/)
1. You can create an account, and add a DNS entry for your device, calling it whatever name is available in the DDNS provider. Let's say I create `mypi`
1. Use the DDNS provider to map `mypi` to your router's public IP address (`73.240.154.193` in the example above). Now you can connect to your Pi using the command
    ```sh
    $ ssh pi@mypi.freeddns.org
    ```
1. Your DDNS provider will probably tell you that DNS changes take time to propagate across the Internet. Can be hours or days.

### Shell aliases (optional)

1. If you don't want to type the whole `ssh` command, you can create a shell alias 
    ```sh
    alias pi="ssh pi@mypi.freeddns.org"
    ```
    or, if you didn't use DDNS
    ```sh
    alias pi="ssh pi@73.240.154.193"
    ```
1. If you're using a Mac and `zsh` (command prompt is `%`) you put the alias in `~/.zshrc`. If you're using `bash` (command prompt is `$`) you put the alias in `~/.bashrc`
1. Now you can connect to you pi just by typing this (as you see me doing in class)
    ```sh
    $ pi
    ```