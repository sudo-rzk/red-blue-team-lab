# Red & Blue Team Lab
A practical cybersecurity lab designed to explore offensive and defensive security operations through controlled attack simulations, network segmentation, web application security testing, log monitoring, threat detection, and incident investigation.

This project demonstrates the relationship between Red Team activities and Blue Team operations within a self-hosted virtualized environment.


## 1. Project Overview

The Red & Blue Team Lab is a personal cybersecurity learning environment built using VirtualBox, MikroTik CHR, Debian Server, Ubuntu, and Kali NetHunter.

The lab is designed to simulate a web server infrastructure where offensive security activities can be performed against a deliberately vulnerable web application, while defensive monitoring components collect and analyze server logs.

The main objective is to understand how activities performed during reconnaissance, vulnerability assessment, exploitation, and post-exploitation can generate observable events that can be investigated from a defensive security perspective.

### Project Objectives

- Build a virtualized network infrastructure using MikroTik CHR.

- Implement network separation and firewall filtering.

- Deploy a web application testing environment using DVWA.

- Configure remote access to the web application through Cloudflare Tunnel.

- Implement centralized log collection using Grafana Alloy and Loki.

- Visualize server activity and create detection alerts using Grafana.

- Perform controlled reconnaissance, vulnerability assessment, exploitation, and post-exploitation activities.

- Investigate attack-related events through server logs and monitoring alerts.

- Understand the relationship between offensive techniques and defensive detection.

## 2. Lab Architecture

The lab uses a combination of physical hardware, virtual machines, and an external testing device.

### Infrastructure Components

| Component | Role |
|---|---|
| Physical Router | Provides internet connectivity to the lab environment. |
| VirtualBox | Hosts the virtualized lab infrastructure. |
| MikroTik CHR | Acts as the virtual router and manages network traffic between lab segments. |
| Ubuntu Host | Acts as the administration and monitoring machine, hosting Grafana and Loki. |
| Debian Server | Hosts the DVWA web application and Grafana Alloy agent. |
| Kali NetHunter | Acts as the external Red Team testing environment. |
| Cloudflare Tunnel | Provides an external access path to the web application. |

### Network Design

The lab uses separate virtual network adapters to connect the virtual machines and host system.

The main network components include:

- **WAN:** Connected through a bridged adapter to the physical router.
- **DMZ:** An isolated virtual network for the Debian web server.
- **Management / Monitoring:** A host-only network connecting the Ubuntu host to MikroTik CHR.
- **Debian server:** is configured within the DMZ network using the address `192.168.10.2`.
- **Ubuntu host:** provides administrative access and hosts the monitoring stack.
- **MikroTik CHR:** controls traffic between the virtual network segments through firewall filtering rules.

### Architecture Diagram

<!-- Add the lab network topology image here. -->

## 3. Monitoring Architecture

The Blue Team monitoring implementation uses three main components:

| Component | Function |
|---|---|
| Grafana Alloy | Collects Apache access logs from the Debian server and forwards them to Loki. |
| Loki | Receives, stores, and indexes log data for querying. |
| Grafana | Visualizes collected logs and evaluates alert rules. |

### Log Flow
<!-- Add the lab grafana topology image here. -->

The monitoring implementation focuses primarily on Apache HTTP access logs.

Custom detection rules are configured to identify selected suspicious HTTP request patterns and web activity.

> **Monitoring Scope:** This implementation is a lightweight log monitoring and alerting solution. Grafana, Loki, and Alloy are not a complete SIEM or EDR platform.


## 4. Red Team Activities

The Red Team environment uses Kali NetHunter to perform controlled testing against the web application.

The documented activities include:
Passive reconnaissance using Subfinder.
- HTTP metadata enumeration using HTTPX.
- Vulnerability assessment and web scanning.
- SQL Injection testing.
- OS Command Injection testing.
- Post-exploitation enumeration.
- Linux permission and service enumeration.
- Web application file-write testing.

The purpose of these activities is to evaluate the application's attack surface and observe the resulting server-side activity.

All testing is conducted against the controlled lab environment.

## 5. Blue Team Activities

The Blue Team focuses on monitoring, detection, and investigation of activities generated during Red Team testing.

The documented activities include:

- Collecting Apache access logs.
- Querying and analyzing HTTP events using Loki.
- Creating custom Grafana alert rules.
- Detecting suspicious HTTP enumeration patterns.
- Identifying potential SQL Injection input patterns.
- Monitoring alert states, including Firing and Resolved.
- Auditing the `www-data` account and its permissions.
- Investigating suspicious file creation and web activity.
- Correlating available logs with Red Team test cases.

The objective is to understand how suspicious activity can be identified through available telemetry and how detection results can support incident investigation.



## 6. Documentation

The documentation is organized into two categories: Main Documentation and Supporting Documentation.

### Main Documentation

These documents describe the infrastructure, configuration, monitoring implementation, and security testing workflow.

| No. | Document | Description |
|---|---|---|
| 01 | [Install MikroTik CHR and Lab Notes](docs/main/01-install-mikrotik-chr-and-lab-notes.pdf) | Preparation and installation of MikroTik CHR for the virtualized lab. |
| 02 | [MikroTik CHR Configuration](docs/main/02-mikrotik-chr-configuration.pdf) | Network interfaces, IP addressing, DNS, and firewall configuration. |
| 03 | [Cloudflare Tunnel on Debian Server](docs/main/03-cloudflare-tunnel-debian-server.pdf) | Tunnel configuration for accessing the web application externally. |
| 04 | [Grafana, Loki, and Alloy Monitoring](docs/main/04-grafana-loki-alloy-monitoring.pdf) | Monitoring stack installation, log collection, dashboards, and alerting. |
| 05 | [Red & Blue Team Testing](docs/main/05-red-blue-team-testing.pdf) | Red Team simulation, Blue Team detection, and incident investigation. |

### Supporting Documentation

| Document | Description |
|---|---|
| [Install and Setup DVWA](docs/supporting/01-install-and-setup-dvwa.pdf) | Additional reference for installing and configuring DVWA as a web application testing target. |

> **Note:** The DVWA installation guide documents an installation performed in Kali Linux. Its instructions should not be treated as a complete record of the Debian Server deployment used in the main lab.



## 7. Documentation Gap and Limitations

Not every infrastructure preparation step was documented during the development of this lab.

### Identified Documentation Gap

**Debian Server Installation on VirtualBox**

The lab uses a Debian Server virtual machine as the web server and log collection agent environment. However, the initial installation and VirtualBox VM creation process for Debian Server were not documented.

The existing documentation begins with later network and service configuration stages.

Therefore, the Debian Server installation process is not included as a completed documentation item in this repository.

### Implementation Limitations

- Monitoring telemetry primarily relies on Apache access logs.
- The monitoring stack is not a full SIEM or EDR implementation.
- Detection rules cover selected HTTP patterns and do not represent comprehensive threat detection.
- Test results are limited to the documented lab scenarios.
- Successful command execution as `www-data` does not demonstrate successful privilege escalation to root.

These limitations distinguish demonstrated results from capabilities that have not been tested or documented.



## 8. Tools and Technologies

| Category | Tools |
|---|---|
| Virtualization | VirtualBox |
| Network Infrastructure | MikroTik CHR |
| Operating Systems | Ubuntu, Debian, Kali NetHunter |
| Web Server | Apache2 |
| Vulnerable Web Application | DVWA |
| Tunneling | Cloudflare Tunnel |
| Log Collection | Grafana Alloy |
| Log Storage | Grafana Loki |
| Visualization & Alerting | Grafana Dashboard |
| Reconnaissance | Nmap, Subfinder, HTTPX |
| Web Security Testing | Nikto, Nuclei, SQL Injection, Command Injection |
| Operating System Enumeration | Linux command-line utilities |



## 9. Project Scope and Safety

This project is intended for educational purposes and controlled cybersecurity testing.

The vulnerable application is deliberately deployed for security experimentation.

The lab should not be exposed as an unrestricted vulnerable service to the public internet. External access, where used, is intended for controlled testing.

Do not use the techniques or configurations documented here against systems without explicit authorization.



## 10. Project Status

The lab infrastructure, monitoring implementation, and documented Red & Blue Team test scenarios have been developed as a practical learning project.

The repository serves as a technical portfolio and a record of the implementation process, test evidence, observations, and identified limitations.

Further improvements may include completing missing installation documentation, expanding telemetry sources, and developing additional detection and investigation scenarios.

---
