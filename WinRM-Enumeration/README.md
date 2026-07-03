# WinRM Enumeration and Security Assessment

## Objective

Assess the security of a Windows Remote Management (WinRM) service running on ports 5985/TCP and 5986/TCP and identify potential security weaknesses such as weak credentials, unauthorized remote access, insecure authentication methods, and excessive user privileges.

## Lab Environment

- Attacker Machine: Kali Linux
- Target Machine: Windows Server
- Service Assessed: Windows Remote Management (WinRM)
- Ports Assessed: 5985/TCP (HTTP), 5986/TCP (HTTPS)

## Target Discovery

Host discovery was performed to identify the target system and determine whether WinRM services were accessible.

### Commands Used

```bash
netdiscover
nmap -sn <target-ip>
nmap -p5985,5986 -sV <target-ip>
```

### Findings

- The target host was successfully identified.
- The system was confirmed to be reachable on the network.
- Port 5985/TCP was found open.
- WinRM services were detected on the target machine.

## Service Enumeration

Service enumeration was performed to identify WinRM services, remote management capabilities, and authentication mechanisms.

### Commands Used

```bash
nmap -p- <target-ip>

nmap -p5985,5986 -sV <target-ip>
```

### Findings

- WinRM services were identified on the target machine.
- Remote management functionality was enabled.
- HTTP (5985) and/or HTTPS (5986) WinRM services were detected.
- Additional Windows services and ports were identified.

### Security Risk

Exposed WinRM services may provide attackers with a remote administration interface if valid credentials are obtained.

## Authentication Testing

Authentication testing was performed to identify valid credentials and determine whether unauthorized access to WinRM services was possible.

### Commands Used

```bash
evil-winrm -i <target-ip> -u user2 -p Password@123

evil-winrm -i <target-ip> -u user2 -H <ntlm-hash>
```

### Findings

- Valid credentials successfully authenticated to WinRM.
- Password-based authentication was supported.
- Hash-based authentication was supported.
- Remote PowerShell access was established.

### Security Risk

Weak or compromised credentials may allow attackers to gain unauthorized remote access to Windows systems through WinRM.

## Remote Access Testing

Remote access testing was performed to determine the level of access available after successful WinRM authentication.

### Commands Used

```bash
evil-winrm -i <target-ip> -u user2 -p Password@123

impacket-psexec domain/user:Password@123@<target-ip>
```

### Findings

- Interactive remote PowerShell access was obtained.
- Administrative actions could be performed depending on user privileges.
- Remote system management capabilities were available.
- Access to system resources and files was possible.

### Security Risk

Unauthorized WinRM access may allow attackers to perform administrative tasks, access sensitive data, and move laterally within a Windows environment.

## Security Impact

The assessment identified multiple security weaknesses that could allow unauthorized remote access to Windows systems through WinRM services.

### Identified Risks

- Valid credentials provided remote access.
- Password-based authentication was supported.
- Hash-based authentication was possible.
- Remote PowerShell access was available.
- Administrative actions could be performed depending on user privileges.

### Potential Impact

An attacker with valid credentials or NTLM hashes could gain remote access to Windows systems, execute commands, access sensitive data, and potentially move laterally within the environment.

## Remediation

The following recommendations can help improve the security of WinRM services:

- Enforce strong password policies.
- Implement multi-factor authentication where possible.
- Restrict WinRM access to authorized administrators only.
- Disable WinRM if it is not required.
- Restrict WinRM access using firewall rules.
- Monitor WinRM authentication and session logs.
- Limit administrative privileges based on the principle of least privilege.
- Keep Windows systems updated with the latest security patches.

## Tools Used

- Nmap
- netdiscover
- Evil-WinRM
- Impacket PsExec
- Microsoft Windows Server
- Kali Linux

## Key Learning

This assessment provided hands-on experience in identifying WinRM services, testing authentication mechanisms, establishing remote PowerShell sessions, and evaluating remote administration capabilities.

The exercise reinforced the importance of strong authentication controls, restricted remote management access, proper privilege management, and continuous monitoring of remote administration services.

## Disclaimer

This assessment was conducted in a controlled lab environment for educational and ethical security testing purposes only.