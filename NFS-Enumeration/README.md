# NFS Enumeration and Security Assessment

## Objective

Assess the security of an NFS service running on port 2049/TCP and identify potential security weaknesses such as exposed NFS shares, unauthorized file access, weak export permissions, sensitive information disclosure, and insecure mount configurations.

## Lab Environment

- Attacker Machine: Kali Linux
- Target Machine: Linux Host
- Protocol Assessed: NFS (2049/TCP)

## Target Discovery

Host discovery was performed to identify the target system and determine whether NFS services were accessible.

### Commands Used

```bash
netdiscover
nmap -sn <target-ip>
nmap -p2049 <target-ip>
```

### Findings

- The target host was successfully identified.
- The system was confirmed to be reachable on the network.
- Port 2049/TCP was found open.
- NFS services were detected on the target machine.

## Service Enumeration

Service enumeration was performed to identify NFS services, RPCbind information, and exported file systems.

### Commands Used

```bash
nmap -p2049 -sC -sV <target-ip>

nmap -p2049,111 -sC <target-ip>

nmap --script nfs* -p2049,111 <target-ip>
```

### Findings

- NFS services were identified on port 2049/TCP.
- RPCbind services were identified on port 111/TCP.
- Exported NFS shares were enumerated.
- File system access permissions were assessed.
- Shared directories available for mounting were identified.

### Security Risk

Misconfigured NFS services may expose shared directories to unauthorized users, leading to information disclosure and unauthorized file access.

## NFS Share Enumeration

NFS share enumeration was performed to identify exported directories and determine whether they were accessible to unauthorized users.

### Commands Used

```bash
nmap --script nfs* -p2049,111 <target-ip>
```

### Findings

- Exported NFS shares were successfully identified.
- Shared directories were accessible for enumeration.
- Export permissions were reviewed.
- Network-wide access permissions were identified.

### Security Risk

Improperly configured NFS exports may allow unauthorized users to discover and access shared resources across the network.

## NFS Mount Testing

NFS mount testing was performed to determine whether exported directories could be mounted and accessed from a remote system.

### Commands Used

```bash
mkdir /tmp/nfs

sudo mount -t nfs <target-ip>:/ /tmp/nfs

ls -la /tmp/nfs
```

### Findings

- Exported NFS shares were successfully mounted.
- Remote file systems became accessible locally.
- Directory structures and files were enumerated.
- Access permissions on mounted resources were assessed.

### Security Risk

Unauthorized NFS mounting may allow attackers to access sensitive files and interact directly with exported resources.

## File Access Testing

File access testing was performed to determine whether files and directories within mounted NFS shares could be viewed, modified, or created.

### Commands Used

```bash
cd /tmp/nfs

ls -la

touch test.txt

ls -la
```

### Findings

- Files and directories within the mounted share were accessible.
- Existing files could be viewed and enumerated.
- New files could be created within writable directories.
- File ownership and permissions were identified.
- Read and write access permissions were assessed.

### Security Risk

Writable NFS shares may allow unauthorized users to create, modify, or delete files, potentially leading to data tampering or system compromise.

## Sensitive Information Discovery

Sensitive information discovery was performed to identify configuration files, user information, and credentials exposed through mounted NFS shares.

### Commands Used

```bash
cd /tmp/nfs/etc

cat /tmp/nfs/etc/shadow

ls /tmp/nfs/etc | grep exports

cat /tmp/nfs/etc/exports
```

### Findings

- Sensitive system configuration files were accessible.
- User account information was identified.
- Password hash information may be exposed.
- NFS export configurations were disclosed.
- Export permissions and access control settings were identified.

### Security Risk

Exposure of sensitive files such as shadow and exports may provide attackers with credential data and information about NFS access controls, increasing the risk of further compromise.

## Security Impact

The assessment identified multiple security weaknesses that could expose sensitive files and allow unauthorized access to NFS resources.

### Identified Risks

- Exported NFS shares were accessible.
- Remote mounting of shared directories was possible.
- Sensitive system files were exposed.
- Weak export permissions increased the risk of unauthorized access.
- Writable shares could allow file modification and data tampering.

### Potential Impact

An attacker could access sensitive files, obtain credential information, modify data, and potentially leverage exposed NFS resources to gain further access within the environment.

## Remediation

The following recommendations can help improve the security of NFS services:

- Restrict NFS exports to authorized hosts only.
- Avoid exporting sensitive directories.
- Implement the principle of least privilege for shared resources.
- Disable write access where not required.
- Use firewall rules to restrict NFS and RPCbind access.
- Regularly review export configurations and permissions.
- Monitor NFS access logs for suspicious activity.
- Keep NFS services updated with the latest security patches.

## Tools Used

- Nmap
- netdiscover
- mount
- NFS NSE Scripts
- Kali Linux
- RPCbind

## Key Learning

This assessment provided hands-on experience in identifying NFS services, enumerating exported shares, mounting remote file systems, and assessing the risks associated with exposed NFS resources.

The exercise reinforced the importance of secure export configurations, proper access controls, restricted permissions, and protecting sensitive files from unauthorized access.

## Disclaimer

This assessment was conducted in a controlled lab environment for educational and ethical security testing purposes only.