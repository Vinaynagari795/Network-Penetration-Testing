# MS-SQL Enumeration and Security Assessment

## Objective

Assess the security of a Microsoft SQL Server service running on port 1433/TCP and identify potential security weaknesses such as weak credentials, excessive database permissions, command execution capabilities, insecure configurations, and unauthorized file access.

## Lab Environment

- Attacker Machine: Kali Linux
- Target Machine: Windows Server
- Service Assessed: Microsoft SQL Server (1433/TCP)

## Target Discovery

Host discovery was performed to identify the target system and determine whether Microsoft SQL Server services were accessible.

### Commands Used

```bash
netdiscover
nmap -sn <target-ip>
nmap -p1433 <target-ip>
```

### Findings

- The target host was successfully identified.
- The system was confirmed to be reachable on the network.
- Port 1433/TCP was found open.
- Microsoft SQL Server services were detected on the target machine.

## Service Enumeration

Service enumeration was performed to identify Microsoft SQL Server versions, instance information, and configuration details.

### Commands Used

```bash
nmap -p1433 -sC -sV <target-ip>
```

### Findings

- Microsoft SQL Server was identified on port 1433/TCP.
- SQL Server version information was disclosed.
- Instance details were identified.
- Host and domain information were disclosed.
- Database service configuration details were collected.

### Security Risk

Exposed service and version information may assist attackers in identifying vulnerabilities, misconfigurations, and potential attack paths.

## Credential Testing

Credential testing was performed to identify weak, default, or reusable credentials that could provide unauthorized access to Microsoft SQL Server.

### Common Credentials Tested

```text
sa:Password@123
sa:sa
admin:admin
msfadmin:msfadmin
bob:bob
```

### Commands Used

```bash
nxc mssql <target-ip> -u user -p pass --continue-on-success

nxc mssql <target-ip> -u user -p pass --continue-on-success --local-auth
```

### Findings

- Weak and default credentials were assessed.
- Valid SQL Server credentials were identified.
- Local authentication was tested successfully.
- Authentication provided access to SQL Server resources.

### Security Risk

Weak credentials can allow attackers to gain unauthorized access to databases, sensitive information, and administrative functions.

## Database Enumeration

Database enumeration was performed to identify available databases and assess access permissions.

### Commands Used

```bash
nxc mssql <target-ip> -u sa -p Password@123 --local-auth -q "SELECT name FROM master.dbo.sysdatabases;"
```

### Findings

- Database names were successfully enumerated.
- System databases were identified.
- User-created databases were discovered.
- Database access permissions were verified.

### Security Risk

Unauthorized database enumeration can expose sensitive business information and assist attackers in identifying valuable targets for further compromise.

## Command Execution Testing

Command execution testing was performed to determine whether SQL Server was configured to allow operating system command execution through xp_cmdshell.

### Commands Used

```bash
impacket-mssqlclient sa:'Password@123'@<target-ip> --windows-auth
```

```sql
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```

### Findings

- SQL Server authentication was successful.
- Administrative privileges were identified.
- xp_cmdshell functionality was assessed.
- Operating system command execution capabilities were evaluated.
- System-level access may be available through SQL Server misconfigurations.

### Security Risk

Misconfigured SQL Server instances with xp_cmdshell enabled may allow attackers to execute operating system commands and potentially gain elevated access to the host system.

## File Upload Testing

File upload testing was performed to determine whether files could be transferred to the target system through SQL Server access.

### Commands Used

```bash
nxc mssql <target-ip> -u sa -p Password@123 --put-file test.py C:\Temp\test.py --local-auth

nxc mssql <target-ip> -u sa -p Password@123 -x "dir C:\Temp\test.py" --local-auth
```

### Findings

- Files were successfully uploaded to the target system.
- Uploaded files were verified on the remote host.
- File system access was confirmed.
- Administrative privileges allowed interaction with system resources.

### Security Risk

Unauthorized file uploads may allow attackers to place malicious tools, scripts, or payloads on the target system, increasing the risk of system compromise.

## Security Impact

The assessment identified multiple security weaknesses that could allow unauthorized access to Microsoft SQL Server resources and the underlying operating system.

### Identified Risks

- Weak or default credentials were accepted.
- Database enumeration was possible.
- Administrative privileges were available.
- xp_cmdshell functionality may allow operating system command execution.
- File uploads to the target system were possible.

### Potential Impact

An attacker could access sensitive databases, execute operating system commands, upload malicious files, and potentially gain full control of the target server through SQL Server misconfigurations.

## Remediation

The following recommendations can help improve the security of Microsoft SQL Server:

- Enforce strong password policies for SQL Server accounts.
- Disable or remove default accounts where possible.
- Restrict administrative privileges to authorized users only.
- Disable xp_cmdshell unless absolutely required.
- Monitor SQL Server authentication and activity logs.
- Restrict file upload capabilities.
- Limit network access to SQL Server services through firewall rules.
- Keep SQL Server updated with the latest security patches.

## Tools Used

- Nmap
- netdiscover
- NetExec (NXC)
- Impacket MSSQL Client
- Microsoft SQL Server
- Kali Linux

## Key Learning

This assessment provided hands-on experience in identifying Microsoft SQL Server services, testing authentication mechanisms, enumerating databases, evaluating command execution functionality, and assessing file transfer capabilities.

The exercise reinforced the importance of strong authentication, least-privilege access controls, secure SQL Server configurations, and restricting operating system command execution through database services.

## Disclaimer

This assessment was conducted in a controlled lab environment for educational and ethical security testing purposes only.