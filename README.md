# Wazuh SIEM Deployment on AWS EC2

## Project Overview

This project documents the deployment of **Wazuh as a security monitoring and SIEM platform on Amazon Web Services (AWS)**.

The goal was to build a cloud-based security monitoring environment, connect an endpoint to the Wazuh server, and confirm that the deployment was generating and displaying security alerts through the Wazuh dashboard.

The project was carried out as a practical cybersecurity lab and is intended to demonstrate hands-on experience with:

- Cloud-based security monitoring
- SIEM deployment
- Endpoint monitoring
- Security alert analysis
- Log collection and security event visibility
- Wazuh agent deployment and validation
- AWS EC2 administration
- Basic threat detection and security operations

> **Note:** This repository documents the implementation and evidence available from the project. Sensitive infrastructure details such as credentials, private keys, and reusable secrets are intentionally excluded.

---

## Objectives

The main objectives of the project were to:

1. Deploy a Wazuh server in an AWS EC2 environment.
2. Configure the cloud instance to host the Wazuh platform.
3. Connect an endpoint to the Wazuh server using the Wazuh agent.
4. Confirm that the agent was communicating successfully with the server.
5. Generate and observe security events through the Wazuh dashboard.
6. Review alert severity levels and understand the resulting security telemetry.
7. Document the deployment as a portfolio project.

---

## Project Architecture

The basic architecture for the lab was:

```text
                         Internet
                            |
                            |
                    +----------------+
                    |    AWS Cloud   |
                    |                |
                    |  EC2 Instance  |
                    |                |
                    | Wazuh Server   |
                    | Wazuh Manager  |
                    | Wazuh Indexer  |
                    | Wazuh Dashboard|
                    +--------+-------+
                             |
                             | Wazuh Agent
                             |
                    +--------v-------+
                    |    Endpoint    |
                    |                |
                    | Monitored Host |
                    +----------------+
```

The EC2 instance hosted the Wazuh environment while an endpoint was connected to the Wazuh server using the Wazuh agent.

---

## AWS Environment

The Wazuh server was deployed as an Amazon EC2 instance.

### EC2 instance observed during the project

| Item | Value |
|---|---|
| Instance name | `mywazuhserver` |
| Instance ID | `i-087d2b84e18b297e3` |
| Instance type | `t3.xlarge` |
| Instance state | Running |
| Availability Zone | `eu-north-1a` |
| Private IPv4 | `172.31.17.200` |
| Public IPv4 | Assigned |
| Purpose | Wazuh security monitoring server |

The public IP address is deliberately not reproduced in this README because infrastructure addresses can change and should not be unnecessarily exposed in a public portfolio repository.

### EC2 evidence

The AWS console confirmed that the Wazuh server instance was running.

![AWS EC2 Wazuh Server](evidence/01-aws-ec2-instance.png)

---

# Implementation

## 1. Provisioning the AWS EC2 Instance

An EC2 instance was provisioned to serve as the Wazuh server.

The instance was configured with sufficient resources for the Wazuh stack and was placed inside an AWS VPC.

The EC2 console showed the instance in a **Running** state after deployment.

At this stage, the main infrastructure task was to make the server reachable while keeping unnecessary exposure to a minimum.

### Security considerations

The EC2 security group should only allow the ports required by the Wazuh deployment and administrative access.

A production deployment should avoid exposing management interfaces to the entire internet. Where possible, administrative access should be restricted to trusted source IP addresses or controlled through a VPN, bastion host, or other secure access mechanism.

---

## 2. Wazuh Server Deployment

The Wazuh platform was deployed on the EC2 server.

The Wazuh platform provides several components that work together to provide security monitoring:

- **Wazuh Manager** – processes security events and applies detection rules.
- **Wazuh Agent** – collects security telemetry from monitored endpoints.
- **Wazuh Indexer** – stores and indexes security event data.
- **Wazuh Dashboard** – provides the web interface used to investigate alerts and security information.

The resulting environment provided a central location for collecting and reviewing endpoint security events.

---

## 3. Agent Deployment

A Wazuh agent was deployed to a monitored endpoint and connected to the Wazuh server.

The agent is responsible for collecting security-relevant information from the endpoint and forwarding it to the Wazuh manager.

Once the agent was connected successfully, it appeared in the Wazuh dashboard as an active agent.

---

## 4. Agent Validation

The Wazuh dashboard was checked after the agent deployment to confirm communication between the endpoint and the Wazuh server.

The dashboard showed:

```text
Active agents:       1
Disconnected agents: 0
```

This confirmed that the deployed agent was actively communicating with the Wazuh environment at the time the screenshot was captured.

![Wazuh Agent and Alerts Dashboard](evidence/02-wazuh-dashboard-agent-alerts.png)

---

# Security Monitoring Results

The Wazuh dashboard provided visibility into security events generated during the monitoring period.

At the time the dashboard evidence was captured, the **Last 24 Hours Alerts** section showed:

| Severity | Alert Count |
|---|---:|
| Critical | 0 |
| High | 0 |
| Medium | 479 |
| Low | 304 |
| **Total** | **783** |

The Wazuh dashboard categorised these alerts according to Wazuh rule levels.

### Rule-level classification shown in the dashboard

```text
Critical severity → Rule level 15 or higher
High severity     → Rule level 12 to 14
Medium severity   → Rule level 7 to 11
Low severity      → Rule level 0 to 6
```

The absence of critical and high alerts in this particular snapshot does **not** mean that the environment was completely secure. It only reflects the alerts observed during the captured monitoring period.

The 479 medium-severity and 304 low-severity alerts provide useful telemetry for investigating endpoint activity, configuration issues, authentication events, and other security-related behaviour.

---

# Alert Analysis

The alert dashboard demonstrates one of the key benefits of a SIEM platform: security events that would otherwise be distributed across individual systems can be collected and presented through a central monitoring interface.

For a SOC-style workflow, the next step would be to investigate individual alerts rather than treating the alert count alone as an indication of risk.

A typical investigation process would be:

```text
Alert
  |
  v
Identify rule
  |
  v
Review affected endpoint
  |
  v
Examine event details
  |
  v
Determine whether activity is expected
  |
  +-------> False Positive / Benign
  |
  +-------> Suspicious Activity
                |
                v
          Investigate further
                |
                v
          Containment / Response
                |
                v
          Document findings
```

This approach is more useful than simply monitoring the number of alerts because alert volume does not automatically equal security risk.

---

# Skills Demonstrated

This project demonstrates practical experience in the following areas:

### Cloud Security

- AWS EC2 deployment
- Cloud-hosted security infrastructure
- AWS security group considerations
- Secure remote administration

### SIEM

- Wazuh deployment
- Centralised security monitoring
- Security alert collection
- Alert severity classification
- Security event investigation

### Endpoint Security

- Wazuh agent deployment
- Endpoint visibility
- Agent/server communication
- Endpoint security telemetry

### Security Operations

- Alert monitoring
- Initial alert triage
- Severity assessment
- Security event investigation workflow
- Basic SOC monitoring concepts

### Documentation

- Infrastructure documentation
- Deployment evidence
- Security monitoring results
- Portfolio-oriented technical reporting

---

# Tools and Technologies

| Category | Technology |
|---|---|
| Cloud | Amazon Web Services (AWS) |
| Compute | Amazon EC2 |
| SIEM / XDR | Wazuh |
| Endpoint Monitoring | Wazuh Agent |
| Security Monitoring | Wazuh Dashboard |
| Log / Event Management | Wazuh Indexer |
| Security Operations | Alert triage and investigation |
| Infrastructure | AWS VPC / Security Groups |

---

# Evidence

The repository contains screenshots captured during the project.

### 01 — AWS EC2 Instance

Shows the Wazuh server running on an AWS EC2 instance.

![EC2 Instance](evidence/01-aws-ec2-instance.png)

### 02 — Wazuh Dashboard

Shows the connected Wazuh agent and the security alerts generated during the monitoring period.

![Wazuh Dashboard](evidence/02-wazuh-dashboard-agent-alerts.png)

---

# Repository Structure

```text
wazuh-aws-siem-project/
│
├── README.md
│
├── evidence/
│   ├── 01-aws-ec2-instance.png
│   └── 02-wazuh-dashboard-agent-alerts.png
│
├── documentation/
│   ├── deployment-notes.md
│   ├── agent-deployment.md
│   └── alert-analysis.md
│
├── architecture/
│   └── wazuh-aws-architecture.png
│
└── screenshots/
    └── README.md
```

### Recommended purpose of each directory

**`evidence/`**  
Contains screenshots and other evidence directly supporting the project.

**`documentation/`**  
Contains detailed implementation notes, troubleshooting records, and analysis.

**`architecture/`**  
Contains architecture diagrams showing how AWS, Wazuh and monitored endpoints interact.

**`screenshots/`**  
Can be used if you later want to separate general walkthrough screenshots from formal evidence.

---

# Security and Privacy Considerations

Before publishing this project publicly on GitHub, review all screenshots and documentation for sensitive information.

Do not commit:

- AWS access keys
- AWS secret keys
- SSH private keys
- Passwords
- Wazuh credentials
- API tokens
- Session tokens
- Private certificates
- Internal hostnames that should remain confidential
- Internal IP addresses where disclosure is inappropriate
- Customer or personal data

For portfolio documentation, it is better to show the configuration and security outcome without exposing credentials or unnecessary infrastructure details.

---

# Lessons Learned

This project provided practical experience in moving from a basic cloud server deployment to an operational security monitoring environment.

One of the most useful parts of the exercise was seeing the relationship between an endpoint agent, the Wazuh server and the dashboard. Deploying the agent is only the first step; the real value comes from being able to collect events, interpret alerts and decide which events require investigation.

The project also reinforced the importance of infrastructure security. A SIEM server itself is a security-sensitive asset, so network exposure, administrative access and credential management need to be considered as part of the deployment rather than after the system is operational.

---

# Possible Improvements

The current implementation can be extended into a more complete SOC laboratory by adding:

- Multiple monitored endpoints
- Windows endpoint monitoring
- Linux endpoint monitoring
- File Integrity Monitoring (FIM)
- Vulnerability detection
- Security Configuration Assessment (SCA)
- Malware detection
- Custom Wazuh detection rules
- MITRE ATT&CK mapping
- Active response
- Integration with threat intelligence sources
- Automated incident response
- Alert dashboards for specific attack techniques
- Log retention and backup controls
- Centralised AWS logging
- CloudTrail integration
- Security monitoring for AWS workloads

A useful next stage would be to simulate controlled security events against the monitored lab endpoints and document how the resulting Wazuh alerts are detected, triaged and investigated.

---

# Portfolio Outcome

This project demonstrates that the implementation was not limited to installing a security tool. It covered the complete basic monitoring workflow:

```text
AWS Infrastructure
        ↓
Wazuh Deployment
        ↓
Agent Deployment
        ↓
Endpoint Visibility
        ↓
Security Events
        ↓
Alert Generation
        ↓
Alert Monitoring
        ↓
Investigation
```

---



---

## Disclaimer

This project was conducted for educational, laboratory and portfolio purposes. All security testing and monitoring activities should be performed only on systems and environments where you have explicit authorization.
