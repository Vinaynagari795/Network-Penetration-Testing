# FTP Enumeration and Security Assessment

## Objective

Assess the security of an FTP service running on port 21 and identify potential security weaknesses such as anonymous access, weak credentials, and insecure file permissions.

## Lab Environment

- Attacker Machine: Kali Linux
- Target Machine: Metasploitable 2
- Protocol Assessed: FTP (21/TCP)

## Target Discovery

Host discovery was performed to identify the target system on the network.

### Commands Used

```bash
netdiscover
nmap -sn <target-ip>
```
### Findings

The target host was successfully identified and confirmed to be reachable on the network.

## Service Enumeration

Service enumeration was performed to identify the FTP service, version information, and possible misconfigurations.

### Commands Used

```bash
nmap -p21 -sC -sV <target-ip>
nmap -p21 -A <target-ip>
nmap -p- --max-rate 10000 <target-ip>
```

### Findings

- Port 21 (FTP) was found open.
- FTP service version information was identified.
- Anonymous login capability was detected.
- Additional service information was collected through Nmap scripts.


## Anonymous Login Testing

Anonymous login testing was performed to determine whether the FTP server allowed access without valid user credentials.

### Commands Used

```bash
ftp <target-ip>
```

### Findings

- Anonymous login was permitted by the FTP service.
- Access to FTP resources was possible without authentication.

### Security Risk

Anonymous access can expose sensitive files and information to unauthorized users.

## Credential Testing

Credential testing was performed to identify weak or default credentials that could provide unauthorized access.

### Common Credentials Tested

```text
admin:admin
ftp:ftp
msfadmin:msfadmin
lisa:lisa
```

### Findings

- Weak/default credentials were accepted by the FTP service.
- Successful authentication allowed access to FTP resources.

### Security Risk

Weak credentials significantly increase the risk of unauthorized access and system compromise.

## File Transfer Testing

File transfer functionality was tested to determine the level of access available through the FTP service.

### Commands Used

```bash
ls
dir
get <filename>
put <filename>
```

### Findings

- Directory contents could be listed using FTP commands.
- Files could be downloaded from the target system.
- File upload functionality was available.
- Read and write permissions were identified on the FTP server.

### Security Risk

If upload permissions are misconfigured, attackers may upload malicious files or scripts, potentially leading to system compromise.

## Brute Force Assessment


A controlled credential attack was performed to identify valid FTP usernames and passwords.

### Tools Used

- Hydra
- NetExec (NXC)

### Commands Used

```bash
hydra -L users.txt -P passwords.txt ftp://<target-ip>
```

### Findings

- Credential testing identified valid login credentials.
- Weak password policies increased the likelihood of successful authentication.

### Security Risk

Attackers can gain unauthorized access through password guessing or brute force attacks when weak credentials are used.


## Security Impact

The assessment identified multiple security weaknesses that could allow unauthorized access to the FTP service.

### Identified Risks

- Anonymous login was enabled.
- Weak/default credentials were accepted.
- File upload and download functionality was available.
- Sensitive information could potentially be exposed.

### Potential Impact

An attacker could gain unauthorized access, retrieve sensitive files, upload malicious content, and potentially use the FTP service as an entry point into the target environment.


## Remediation

The following recommendations can help improve the security of the FTP service:

- Disable anonymous FTP access if it is not required.
- Enforce strong password policies.
- Remove default accounts and credentials.
- Restrict file upload permissions to authorized users only.
- Implement account lockout policies to prevent brute force attacks.
- Monitor FTP authentication and access logs regularly.
- Use secure alternatives such as SFTP or FTPS when possible.

## Tools Used

- Nmap
- FTP Client
- Hydra
- NetExec (NXC)
- Kali Linux

## Key Learning

This assessment provided hands-on experience in enumerating FTP services, identifying misconfigurations, testing credentials, and evaluating the security risks associated with insecure FTP deployments.

## Disclaimer

This assessment was conducted in a controlled lab environment for educational and ethical security testing purposes only.