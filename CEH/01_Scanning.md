# **Scanning**
---
## **Basic Scan**
- `ping <host address>` *think sonar ; icmp echo*
    - ***EX of ping sweep script (.sh)***

            #!/bin/bash
            for i in {1..254} 
            do
                ping -c 1 10.0.0.$i | \
                grep ^64 | \
                awk '{print $4}' | \
                cut -d ":" -f 1 & \
            done 

            ## note: `&` tells it to run in back which means....bash bash go fast fast
---
# **Scanning with nmap**
***Always use `--send-ip`***

## **Types of Scans**
- **Stealth Scan:**
    - Stealth scan switch tag `-sS`
    - This is the default nmap scan.
    - Scans by sending `SYN` packets to a port, if the port responds with `SYN ACK` packet, we drop the connection. This simply gives a basic map of the host network, while not alerting anyone... possibly.

- **Full Connect Scan**
    - Full connect scan switch tag `-sT`
    - This scan performs a full TCP handshake, which means; 
        - **Step1**: attacker sends a `SYN` packet.
        - **Step2**: the live ports recieve the packet then send back a `SYN ACK` packet.
        - **Step3**: attacker recieves `SYN ACK` packet, then sends an `ACK` packet to the live ports, thus completeing the *Three way handshake*.

- **UDP Scan**
    - UDP scan switch tag `-sU`
    - ***IT TAKES FOREVER***

- **ACK Scan**
    - ACK scan switch tag `-sA`
    - `ACK` scans help map firewalls.
    - Returns weather a port is being filtered or not.
    - Attacker sends an `ACK` packet to a target, attacker either recieves an `RST` packet ,icmp error codes, or nothing. If the port is filtered, the attacker will recieve error codes or nothing.

- **Xmas Scan**
    - Xmas scan switch tag `-sX`
    - this scan sets the `FIN`, `PSH`, and `URG` flags. Lighting the packet up like a christmas tree.
        - **FIN**: alerts that connection is done
        - **PSH**: push this ahead dont buffer
        - **URG**: urgent!! go go go!!!
    - ***FIN SCANS ARE GOOD FOR SLIPPING THROUGH NON-STATFUL FIREWALLS***

## **Performance Enhancers**
- **Fragmentation**
    - Fragmentation switch tag `-F`.
    - help slip through firewall or ids, not getting enough of the packet to hit a rule.

- **Decoys**
    - Decoy switch tag `-D`
    - makes it seem like attacks are coming from another machine.
    - Show activity from you and the decoy

- **Spoofing**
    *set network interface with the `-e` switch first*
    - Spoofing switch tag `-S`
    - spoofing is like decoy except it completly hides the attackers ip and makes it seem like there is only on ip active.

- **Timing**
    - Timing switch tag `-T<num>`
    - allows to set the intesity of scan, fast is aggressive and slow is stealthy.
    - **Options**
        - `T0` paranoid
        - `T1` sneaky
        - `T2` polite
        - `T3` normal
        - `T4` aggressive
        - `T5` insane

- **Port**
    - Port switch tag `-p`
    - expects ports seperated by commas
    - allows to specify port/s
    - can specify open `--open`
    - set port range port_start`-`port_stop
    - scan all ports `-p-`
    - if using more than one port scanning, specify which ports each should scan.

            ##Example
            root@host:~$ nmap -sU -sT -p U:443,T:80 <ip address>

- **OS Detection**
    - OS Detection switch tag `-O`
    - tries to guess targets operating system
    - *its a guess, remember that*

- **Version Detection**
    - Vesion Detection switch tag `-sV`
    - gets version of service running on target ports.
    - `--version-intensity` the higher the intensity the better it can get the version, but also better chance of tripping alarms.

- **Acessing Scripting Engine**
    - Scripting switch `--script`
    - access the nmap scripts library
    - runs scan then attempts script on target.
    - use key words for scripts;
        - `vuln`
        - `exploit`
        - etc..

- **OUTPUT parameters**
    - look these up in the man pages

- **Input ips from file**
    - switch tag `-il <file.name`
    - read ips from file list
---
## **Tools**
- `nmap` --cli
- `zenmap1` --gui
- `hping3` --cli
- `netdiscover` --cli
- `wireshark` *packet sniffer* --gui
- `xsltproc` *format xml* --cli

## **Services**
- **NSEDocs** nmap scripting engine