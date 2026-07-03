# SMB Enumeration and Security Assessment

## Objective

Assess the security of an SMB service running on port 445 and identify potential security weaknesses such as null sessions, weak credentials, exposed shares, and excessive file permissions.

## Lab Environment

- Attacker Machine: Kali Linux
- Target Machine: Metasploitable 2
- Protocol Assessed: SMB (445/TCP)

## Target Discovery

Host discovery was performed to identify the target system on the network.

### Commands Used

```bash
netdiscover
nmap -sn <target-ip>
nxc smb <target-ip>
```
### Findings

- The target host was successfully identified.
- The system was confirmed to be reachable on the network.
- SMB services were detected on the target machine.

## Service Enumeration

Service enumeration was performed to identify the SMB service, version information, operating system details, and possible misconfigurations.

### Commands Used

```bash
nmap -p445 -sC -sV <target-ip>
nmap -p445 -A <target-ip>
```
### Findings

- Port 445 (SMB) was found open.
- Samba service version information was identified.
- The target operating system was identified.
- Workgroup information was disclosed.
- SMB configuration details were collected.

### Security Risk

Information disclosure through SMB enumeration can assist attackers in profiling the target environment and identifying potential attack paths.

## Null Session Testing

Null session testing was performed to determine whether the SMB service allowed anonymous access without valid user credentials.

### Commands Used

```bash
rpcclient -U "" -N <target-ip>
enum4linux <target-ip>
smbclient -L //<target-ip>
smbmap -H <target-ip>
```

### Findings

- Anonymous SMB access was permitted.
- User account information was disclosed.
- Shared folders were enumerated.
- Operating system information was identified.
- Group and workgroup information were disclosed.

### Security Risk

Null sessions can expose sensitive information to unauthenticated users, assisting attackers during reconnaissance and enumeration activities.

## SMB Share Enumeration

SMB share enumeration was performed to identify accessible shared resources and determine the level of access available to the user.

### Commands Used

```bash
smbclient -L //<target-ip>
smbclient //<target-ip>/tmp
```

### Findings

- Shared folders were successfully enumerated.
- Accessible SMB shares were identified.
- Directory contents could be listed.
- Read and write permissions were assessed.

### Security Risk

Improperly configured SMB shares may expose sensitive data and allow unauthorized users to access network resources.

## Credential Testing

Credential testing was performed to identify weak, default, or blank credentials that could provide unauthorized access to SMB resources.

### Common Credentials Tested

```text
admin:admin
ftp:ftp
msfadmin:msfadmin
lisa:lisa
blank username and password
```

### Findings

- Weak and default credentials were tested against the SMB service.
- Blank credentials were accepted in the lab environment.
- Successful authentication provided access to SMB shares and resources.

### Security Risk

Weak, default, or blank credentials can allow attackers to gain unauthorized access to sensitive files and network resources.

## File Access Testing

File access testing was performed to determine the level of access available within SMB shares and assess the ability to read, download, and upload files.

### Commands Used

```bash
ls
dir
get <filename>
mget *
put <filename>
```

### Findings

- Directory contents could be listed successfully.
- Files could be downloaded from SMB shares.
- Multiple files could be downloaded using bulk download functionality.
- File upload functionality was available.
- Read and write permissions were identified on accessible shares.

### Security Risk

Misconfigured SMB shares may allow unauthorized users to access, modify, or upload files, potentially leading to data exposure or system compromise.

## Brute Force Assessment

A controlled credential attack was performed to identify valid SMB usernames and passwords.

### Tools Used

- Hydra
- NetExec (NXC)

### Commands Used

```bash
nxc smb <target-ip> -u user -p pass --continue-on-success

hydra -L users.txt -P passwords.txt <target-ip> smb
```

### Findings

- Credential testing identified valid SMB credentials.
- Weak password policies increased the likelihood of successful authentication.
- Successful authentication provided access to SMB resources.

### Security Risk

Weak passwords can allow attackers to gain unauthorized access to SMB services through password guessing and brute force attacks.

## Security Impact

The assessment identified multiple security weaknesses that could allow unauthorized access to SMB resources and sensitive information.

### Identified Risks

- Null sessions were permitted.
- Shared resources were exposed.
- Weak and default credentials were accepted.
- File upload and download functionality was available.
- System and user information were disclosed.

### Potential Impact

An attacker could enumerate users, access sensitive files, modify shared content, and potentially use SMB as an entry point for further attacks within the environment.

## Remediation

The following recommendations can help improve the security of the SMB service:

- Disable anonymous SMB access and null sessions.
- Enforce strong password policies.
- Remove default and unused accounts.
- Restrict access to SMB shares based on the principle of least privilege.
- Disable unnecessary SMB shares.
- Enable SMB signing where appropriate.
- Monitor SMB authentication and access logs regularly.
- Keep Samba and SMB services updated with the latest security patches.

## Tools Used

- Nmap
- NetExec (NXC)
- rpcclient
- enum4linux
- smbclient
- smbmap
- Hydra
- Kali Linux

## Key Learning

This assessment provided hands-on experience in enumerating SMB services, identifying null sessions, discovering shared resources, testing credentials, and assessing the risks associated with insecure SMB configurations.

The exercise reinforced the importance of proper authentication, access control, share permissions, and secure SMB configurations.

## Disclaimer

This assessment was conducted in a controlled lab environment for educational and ethical security testing purposes only.