# RDP Enumeration and Security Assessment

## Objective

Assess the security of a Remote Desktop Protocol (RDP) service running on port 3389/TCP and identify potential security weaknesses such as weak credentials, unauthorized remote access, pass-the-hash abuse, and excessive user privileges.

## Lab Environment

- Attacker Machine: Kali Linux
- Target Machine: Windows Server
- Service Assessed: Remote Desktop Protocol (RDP)
- Port Assessed: 3389/TCP

## Target Discovery

Host discovery was performed to identify the target system and determine whether RDP services were accessible.

### Commands Used

```bash
netdiscover

nmap -sn <target-ip>

nmap -p3389 -sV <target-ip>
```

### Findings

- The target host was successfully identified.
- The system was reachable on the network.
- Port 3389/TCP was found open.
- Remote Desktop services were detected on the target machine.

## Service Enumeration

Service enumeration was performed to identify RDP services, remote access capabilities, and authentication requirements.

### Commands Used

```bash
nmap -p3389 -sV <target-ip>
```

### Findings

- Remote Desktop Protocol (RDP) service was identified.
- Remote graphical access was enabled.
- Authentication was required before access was granted.
- The target system exposed remote administration functionality.

### Security Risk

Exposed RDP services may provide attackers with a remote administration interface if valid credentials are obtained.

## Authentication Testing

Authentication testing was performed to identify valid credentials and determine whether unauthorized access to RDP services was possible.

### Commands Used

```bash
xfreerdp /v:<target-ip> /u:user3 /p:Password@123

xfreerdp /v:<target-ip> /u:user3 /pth:<ntlm-hash>
```

### Findings

- Valid credentials successfully authenticated to RDP.
- Password-based authentication was supported.
- Pass-the-Hash authentication was tested.
- Remote desktop access was established.
- Successful authentication depended on valid user permissions and Remote Desktop access rights.

### Security Risk

Weak or compromised credentials may allow attackers to gain unauthorized graphical access to Windows systems through RDP.

## Remote Access Testing

Remote access testing was performed to determine the level of access available after successful RDP authentication.

### Commands Used

```bash
xfreerdp /v:<target-ip> /u:user3 /p:Password@123

xfreerdp /v:<target-ip> /u:user3 /pth:<ntlm-hash>
```

### Findings

- Interactive desktop access was obtained.
- Administrative actions could be performed depending on user privileges.
- Access to applications, files, and system resources was possible.
- Remote system management capabilities were available.

### Security Risk

Unauthorized RDP access may allow attackers to control systems remotely, access sensitive data, and move laterally within the environment.

## Security Impact

The assessment identified multiple security weaknesses that could allow unauthorized remote access through RDP services.

### Identified Risks

- Valid credentials provided remote desktop access.
- Password-based authentication was supported.
- Pass-the-Hash attacks may be possible under specific conditions.
- Remote graphical access was available.
- Administrative actions could be performed depending on user privileges.

### Potential Impact

An attacker with valid credentials or NTLM hashes could gain remote access, execute actions as the authenticated user, access sensitive information, and potentially move laterally within the environment.

## Remediation

The following recommendations can help improve the security of RDP services:

- Enforce strong password policies.
- Implement multi-factor authentication.
- Restrict RDP access to authorized users only.
- Limit RDP exposure using firewall rules.
- Disable RDP if it is not required.
- Monitor RDP login attempts and session activity.
- Apply the principle of least privilege.
- Keep Windows systems updated with the latest security patches.

## Tools Used

- Nmap
- netdiscover
- xFreeRDP
- NetExec (CrackMapExec)
- Microsoft Windows Server
- Kali Linux

## Key Learning

This assessment provided hands-on experience in identifying RDP services, testing authentication mechanisms, performing remote desktop access, and evaluating the security implications of remote administration services.

The exercise reinforced the importance of strong authentication controls, restricted remote access, proper privilege management, and monitoring of RDP activity.

## Disclaimer

This assessment was conducted in a controlled lab environment for educational and ethical security testing purposes only.