# TP 6 - Services réseau

- [TP 6 - Services réseau](#tp-6---services-réseau)
  - [Exercice 1](#exercice-1)
  - [Exercice 2](#exercice-2)
  - [Exercice 3](#exercice-3)
  - [Exercice 5](#exercice-5)
  - [Exercice 6](#exercice-6)

## Exercice 1

| Nom de réseau | Adresse de sous-réseau | Broadcast    | Premiere Adresse | Dernière Adresse |
| ------------- | ---------------------- | ------------ | ---------------- | ---------------- |
| 1             | 172.16.0.0             | 172.16.0.63  | 172.16.0.1       | 172.16.0.62      |
| 2             | 172.16.0.64            | 172.16.0.127 | 172.16.0.65      | 172.16.0.126     |
| 3             | 172.16.0.128           | 172.16.0.191 | 172.16.0.129     | 172.16.0.190     |
| 4             | 172.16.0.192           | 172.16.0.255 | 172.16.0.193     | 192.16.0.254     |
| 5             | 172.16.1.0             | 172.16.1.63  | 172.16.1.1       | 172.16.1.62      |
| 6             | 172.16.1.64            | 172.16.1.127 | 172.16.1.65      | 172.16.1.126     |
| 7             | 172.16.1.128           | 172.16.1.159 | 172.16.1.129     | 172.16.1.158     |

## Exercice 2

2. L'interface **lo** corresponds à l'interface de loopback.
3. Pour faire cela il suffit de faire la commande suivante:

   ```BASH
    User@localhost:~$ sudo apt purge cloud-init
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    The following packages were automatically installed and are no longer required:
    eatmydata libeatmydata1 python-babel-localedata python3-babel python3-jinja2 python3-json-pointer python3-jsonpatch python3-jsonschema python3-markupsafe
    python3-pyrsistent
    Use 'sudo apt autoremove' to remove them.
    The following packages will be REMOVED:
    cloud-init* ubuntu-server-minimal*
    0 upgraded, 0 newly installed, 2 to remove and 0 not upgraded.
    After this operation, 2,679 kB disk space will be freed.
    Do you want to continue? [Y/n] y
    (Reading database ... 161019 files and directories currently installed.)
    Removing ubuntu-server-minimal (1.481) ...
    Removing cloud-init (22.2-0ubuntu1~22.04.3) ...
    Processing triggers for man-db (2.10.2-1) ...
    (Reading database ... 160688 files and directories currently installed.)
    Purging configuration files for cloud-init (22.2-0ubuntu1~22.04.3) ...
    dpkg: warning: while removing cloud-init, directory '/etc/cloud/cloud.cfg.d' not empty so not removed
    Processing triggers for rsyslog (8.2112.0-2ubuntu2.2) ...
    ```

4. Pour faire cela il suffit de rentrer la commande suivante sur le serveur et le client:

   ```BASH
   sudo hostnamectl set-hostname <hostname>
   ```

   So from that, my serveur will be named $teapot$, while my client will be named $teacup$.  
   Note: le nom du client a bien évidemment été changer pour la suite du TP.

## Exercice 3

1. 

```BASH
User@teapot:~$ systemctl status isc-dhcp-server
× isc-dhcp-server.service - ISC DHCP IPv4 server
     Loaded: loaded (/lib/systemd/system/isc-dhcp-server.service; enabled; vendor preset: enabled)
     Active: failed (Result: exit-code) since Mon 2022-10-03 09:36:12 UTC; 16s ago
       Docs: man:dhcpd(8)
    Process: 1927 ExecStart=/bin/sh -ec      CONFIG_FILE=/etc/dhcp/dhcpd.conf;      if [ -f /etc/ltsp/dhcpd.conf ]; then CONFIG_FILE=/etc/ltsp/dhcpd.conf; fi;      [ >   Main PID: 1927 (code=exited, status=1/FAILURE)
        CPU: 13ms

Oct 03 09:36:12 teapot dhcpd[1927]: 
Oct 03 09:36:12 teapot dhcpd[1927]: If you think you have received this message due to a bug rather
Oct 03 09:36:12 teapot dhcpd[1927]: than a configuration issue please read the section on submitting
Oct 03 09:36:12 teapot dhcpd[1927]: bugs on either our web page at www.isc.org or in the README file
Oct 03 09:36:12 teapot dhcpd[1927]: before submitting a bug.  These pages explain the proper
Oct 03 09:36:12 teapot dhcpd[1927]: process and the information we find helpful for debugging.
Oct 03 09:36:12 teapot dhcpd[1927]: 
Oct 03 09:36:12 teapot dhcpd[1927]: exiting.
Oct 03 09:36:12 teapot systemd[1]: isc-dhcp-server.service: Main process exited, code=exited, status=1/FAILURE
Oct 03 09:36:12 teapot systemd[1]: isc-dhcp-server.service: Failed with result 'exit-code'.
```

2. Il suffit de modifier le fichier /etc/netplan/50-cloud-init.yaml

```BASH
  GNU nano 6.2                                                               50-cloud-init.yaml                                                                        
# This file is generated from information provided by the datasource.  Changes
# to it will not persist across an instance reboot.  To disable cloud-init's
# network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        ens192:
            dhcp4: true
            match:
                macaddress: 00:50:56:89:48:d1
        ens224:
            addresses:
                - 192.168.100.1/24
    version: 2
```

3. Il suffit de tapper la commande suivante: **sudo cp dhcpd.conf dhcpd.conf.bak** puis de modifier le fichier original.  

   A quoi correspondent les deux premières lignes ?  
  
   Elles correspondent au temps minimal et maximal qu'une adresse ip peux être attribuer.

4. Il suffit de remplacer les valeurs par celles-ci: 

```BASH
INTERFACESv4="ens224"
INTERFACESv6=""
```

5. Une fois le serveur redémarrer, le status nous affiche cela:

```BASH
User@teapot:~$ sudo systemctl restart isc-dhcp-server
User@teapot:~$ systemctl status isc-dhcp-server
● isc-dhcp-server.service - ISC DHCP IPv4 server
     Loaded: loaded (/lib/systemd/system/isc-dhcp-server.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2022-10-06 06:34:21 UTC; 19s ago
       Docs: man:dhcpd(8)
   Main PID: 1747 (dhcpd)
      Tasks: 4 (limit: 1631)
     Memory: 4.5M
        CPU: 15ms
     CGroup: /system.slice/isc-dhcp-server.service
             └─1747 dhcpd -user dhcpd -group dhcpd -f -4 -pf /run/dhcp-server/dhcpd.pid -cf /etc/dhcp/dhcpd.conf ens224

Oct 06 06:34:21 teapot dhcpd[1747]: PID file: /run/dhcp-server/dhcpd.pid
Oct 06 06:34:21 teapot dhcpd[1747]: Wrote 0 leases to leases file.
Oct 06 06:34:21 teapot sh[1747]: Wrote 0 leases to leases file.
Oct 06 06:34:21 teapot dhcpd[1747]: Listening on LPF/ens224/00:50:56:89:5f:a1/192.168.100.0/24
Oct 06 06:34:21 teapot sh[1747]: Listening on LPF/ens224/00:50:56:89:5f:a1/192.168.100.0/24
Oct 06 06:34:21 teapot sh[1747]: Sending on   LPF/ens224/00:50:56:89:5f:a1/192.168.100.0/24
Oct 06 06:34:21 teapot sh[1747]: Sending on   Socket/fallback/fallback-net
Oct 06 06:34:21 teapot dhcpd[1747]: Sending on   LPF/ens224/00:50:56:89:5f:a1/192.168.100.0/24
Oct 06 06:34:21 teapot dhcpd[1747]: Sending on   Socket/fallback/fallback-net
Oct 06 06:34:21 teapot dhcpd[1747]: Server starting service.
User@teapot:~$
```

7. DHCPDISCOVER correspond au fait qu'une machine vient de rentrer en contact avec le serveur DHCP.  
DHCPOFFER correspond au fait que le serveur DHCP propose au client une adresse ip.  
DHCPREQUEST est le fait que le client demande l'ip proposé.  
DHCPACK est le fait que cette ip est prise par le client.  
Le client a bien l'ip proposé par le serveur, dans notre cas **192.168.100.100**.

8. Le fichier **/var/lib/dhcp/dhcpd.leases** contient les logs des leases proposé et ayant été proposé avec leur date. La commande **dhcp-lease-list** affiche l'ensemble des leases en cours avec l'ip le nom de la machine et sa validité.

9. Les machines peuvent bien communiquer:

```BASH
User@teapot:~$ ping 192.168.100.100
PING 192.168.100.100 (192.168.100.100) 56(84) bytes of data.
64 bytes from 192.168.100.100: icmp_seq=1 ttl=64 time=0.372 ms
64 bytes from 192.168.100.100: icmp_seq=2 ttl=64 time=0.497 ms
^C
--- 192.168.100.100 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1020ms
rtt min/avg/max/mdev = 0.372/0.434/0.497/0.062 ms
```

## Exercice 5

2. La configuration est la suivante:

```BASH
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.

         forwarders {
                1.1.1.1;8.8.8.8;
         };

        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        dnssec-validation auto;

        listen-on-v6 { any; };
};
``` 

4. En nous rendant sur wikipedia, Cette page apparait:

```BASH
#←←←                                                                                                                         Wikipédia, l'encyclopédie libre (p1 of 16)
   #alternate Images du jour de Wikipédia Flux d’articles en vedette de Wikipédia Regards sur l'actualité de la Wikimedia, et d'ailleurs Wikimag Wikipédia (fr)

   Aller au contenu
   [ ]

   Afficher / masquer la barre latérale Wikipédia l'encyclopédie libre
   Rechercher
   ____________________ Rechercher Lire

     * Créer un compte

   [ ] Outils personnels
   Créer un compte
   Se connecter

   Pages pour les contributeurs déconnectés en savoir plus
     * Discussion
     * Contributions
```

## Exercice 6

2. Voici le fichier apres modification:

```BASH
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     tpadmin.local. root.tpadmin.local. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      tpadmin.local.
@       IN      A       192.168.100.20
@       IN      AAAA    ::1
```

3. Le fichier de configuration ressemble à ça.

```BASH
;
; BIND reverse data file for local loopback interface
;
$TTL    604800
@       IN      SOA     tpadmin.local. root.tpadmin.local. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      tpadmin.local.
20      IN      PTR     teapot.tpadmin.local.
100     IN      PTR     client.tpadmin.local.
```

5. Une fois le serveur Bind relancer, le client peux pinger google.fr:

```BASH
User@client:~$ ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=53 time=10.7 ms
^C
--- 1.1.1.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 10.729/10.729/10.729/0.000 ms
User@client:~$ ping google
ping: google: Temporary failure in name resolution
User@client:~$ ping google.fr
PING google.fr (172.217.18.195) 56(84) bytes of data.
64 bytes from par10s38-in-f3.1e100.net (172.217.18.195): icmp_seq=1 ttl=112 time=8.72 ms
64 bytes from ham02s14-in-f195.1e100.net (172.217.18.195): icmp_seq=2 ttl=112 time=8.73 ms
64 bytes from par10s38-in-f3.1e100.net (172.217.18.195): icmp_seq=3 ttl=112 time=9.49 ms
64 bytes from par10s38-in-f3.1e100.net (172.217.18.195): icmp_seq=4 ttl=112 time=8.88 ms
64 bytes from par10s38-in-f3.1e100.net (172.217.18.195): icmp_seq=5 ttl=112 time=8.65 ms
64 bytes from par10s38-in-f3.1e100.net (172.217.18.195): icmp_seq=6 ttl=112 time=8.71 ms
64 bytes from ham02s14-in-f195.1e100.net (172.217.18.195): icmp_seq=7 ttl=112 time=9.14 ms
64 bytes from ham02s14-in-f195.1e100.net (172.217.18.195): icmp_seq=8 ttl=112 time=9.56 ms
^C
--- google.fr ping statistics ---
8 packets transmitted, 8 received, 0% packet loss, time 7012ms
rtt min/avg/max/mdev = 8.649/8.985/9.563/0.345 ms
User@client:~$ 
```
