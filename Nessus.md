# Vulnerability Assessment: Nessus

## Overview
Nessus is a widely used vulnerability scanner developed by Tenable. It identifies vulnerabilities, misconfigurations, and compliance issues across a network, helping organizations proactively secure their infrastructure.

## Key Features
- Network and host scanning
- Comprehensive vulnerability database
- Configurable scan policies
- Compliance checks
- Exportable reports (PDF, HTML, CSV)

## Installation
1. Download Nessus from [Tenable](https://www.tenable.com/products/nessus)
2. Install following your OS instructions
3. Start the Nessus service

## Basic Usage
1. **Access the Web Interface**
   - Navigate to `https://localhost:8834`
2. **Create a Scan**
   - Go to Scans → New Scan → Select Template (Basic Network Scan)
3. **Configure Targets**
   - Enter IP addresses or ranges
4. **Run Scan**
   - Click Launch and monitor progress
5. **Analyze Results**
   - View detected vulnerabilities, severity levels, and suggested remediations

## Tips
- Regularly update the Nessus plugins for latest vulnerabilities
- Use scan policies to tailor scans for different network segments
- Export reports for auditing and compliance documentation