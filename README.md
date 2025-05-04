# Overview 
This repository contains an academic essay titled "The Shift to Cloud Computing - Implications for Enterprises and Threat Detection with AWS", as part of my capstone project for Cyber Operations at the University of Arizona. 

This project involves discussion about the implications of enterprises wanting to shift to the cloud from on premise solutions, primarily focusing on the implications of misconfigurations made in the cloud. The dangers of misconfigurations are demonstrated through the deployment of an EC2 instance with a security group open to all inbound and outbound internet traffic, custom logging with a PowerShell Script, a CloudWatch Agent, GuardDuty, Event Bridge, and Grafana. 

# Methodology
**EC2 Instance**:
The EC2 instance was opened to all internet traffic, and experienced thousands of attempts for connection while running. Its security group was configured to allow it to be more discoverable to those scanning for open EC2 instances running. 

**PowerShell Script**:
Since the EC2 instance experiences many attempts for connection, each failed login attempt via RDP will be saved under Windows Security Event 4625. The PowerShell script takes newly added Windows Security Event Logs, requests the IpGeolocation API for geolocation information (latitude, longitude, country, city) and creates a new .jsonl file with all of the security event log information + geolocation information. 

**CloudWatch**:
A CloudWatch Agent is installed on the instance, and configured to monitor logs from two places: the regular Windows Security Event Logs, and the logs enriched with the IPGeolocation API. Once a new log is created, it is uploaded to a CloudWatch Log Stream. 

**GuardDuty**:
GuardDuty is a service native to AWS for threat detection. In this project it is used to monitor for port scanning, as an open EC2 instance will be scanned for the ports it is running in order for attackers to carry out reconnaissance. 

**Event Bridge**:
Since the port scanning logs from GuardDuty are not exported to CloudWatch by default, AWS Event Bridge can be utilized to configure an Event Pattern for exporting a new GuardDuty port scan to CloudWatch logs.

**Grafana**:
To create visualizations from the CloudWatch Logs, Grafana can be connected to the CloudWatch database. Hosted on an ubuntu EC2 instance, Grafana is able to query the CloudWatch Logs in order to create meaningul visualizations of the observed network traffic from the instance. 

# Purpose
The purpose of this project is to demonstrate the dangers of misconfigured resources in the cloud, and how tools in AWS can be used for threat detection and logging of these resources. 
