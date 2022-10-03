# TP 6 - Services réseau

- [TP 6 - Services réseau](#tp-6---services-réseau)
  - [Exercice 1](#exercice-1)
  - [Exercice 2](#exercice-2)
  - [Exercice 3](#exercice-3)

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