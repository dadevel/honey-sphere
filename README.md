# HoneySphere

A simple VMware vSphere honeypot powered by [Caddy](https://caddyserver.com/).
It is intended to deceive automatic scanners and careless ransomware groups, but does not withstand closer inspection.

![Screenshot](./screenshot.png)

## Setup

Download the [Caddyfile](./Caddyfile) and start Caddy.

~~~ bash
curl -LO https://github.com/dadevel/honey-sphere/raw/refs/heads/main/Caddyfile
caddy run
~~~

Use [nuclei](https://github.com/projectdiscovery/nuclei-templates/) to verify that the honeypot is detected as a vSphere instance.

~~~ bash
nuclei -u https://localhost -exclude-tags network
~~~

Set up some mechanism that monitors the Caddy logs and sends an alert when someone tries to log in.

Leave breadcrumbs in your environment:

- set up a DNS record that points to the honeypot (e.g. `vcenter1.corp.local`)
- add a password for *root@vsphere.local* to your central password manager
- put a PowerShell script on a world-readable SMB share with credentials for *monitor@vsphere.local*
- create an Active Directory group named *vSphere Admins*, add some of your admins as members and put a reference to `vcenter1.corp.local` in the description

