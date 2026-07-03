# SNMP Enumeration and Security Assessment

## Objective

Assess the security of an SNMP service running on ports 161/UDP and 162/UDP and identify potential security weaknesses such as default community strings, weak community names, insecure SNMP versions, information disclosure, and exposed management data.

## Lab Environment

- Attacker Machine: Kali Linux
- Target Machine: Linux Host
- Protocol Assessed: SNMP (161/UDP, 162/UDP)

## Target Discovery

Host discovery was performed to identify the target system and determine whether SNMP services were accessible.

### Commands Used

```bash
netdiscover
nmap -sn <target-ip>
nmap -sU -p161 <target-ip>
```

### Findings

- The target host was successfully identified.
- The system was confirmed to be reachable on the network.
- UDP port 161 was found accessible.
- SNMP services were detected on the target machine.

## Service Enumeration

SNMP enumeration was performed to identify service versions, community strings, and exposed management information.

### Commands Used

```bash
nmap -sU -p161 -sV <target-ip>
snmp-check <target-ip>
```

### Findings

- SNMP service was detected on the target machine.
- Service version information was identified.
- Community string information was assessed.
- Host and operating system details were disclosed.

### Security Risk

SNMP information disclosure can reveal valuable system and network details that may assist attackers during reconnaissance activities.

## Community String Testing

Community string testing was performed to identify default, weak, or misconfigured community names that could allow unauthorized access to SNMP information.

### Common Community Strings Tested

```text
public
private
admin
security
cisco
```

### Findings

- Default and weak community strings were assessed.
- The community string "public" may provide read-only access to SNMP information.
- Weak community strings increase the risk of unauthorized information disclosure.
- Community string exposure may allow attackers to enumerate system information.

### Security Risk

Weak or default community strings can expose sensitive device information and assist attackers during reconnaissance activities.

## SNMP Walk Enumeration

SNMP walk enumeration was performed to retrieve management information from the target system using identified community strings.

### Commands Used

```bash
snmpwalk -v1 -c public <target-ip>

snmpwalk -v1 -c public <target-ip> NET-SNMP-EXTEND-MIB::nsExtendObjects
```

### Findings

- System information was successfully retrieved.
- Operating system details were identified.
- Hostname and network information were disclosed.
- Management Information Base (MIB) objects were enumerated.
- Extended SNMP objects were accessible.

### Security Risk

SNMP walk enumeration can expose detailed information about the target system, including configuration data, installed services, and operational details.

## Sensitive Information Discovery

Sensitive information discovery was performed to identify usernames, passwords, application details, scripts, and other valuable data exposed through SNMP.

### Information Searched For

- Usernames
- Passwords
- Application names
- Software versions
- Scripts
- Commands
- Network configuration details
- Data transfer information

### Findings

- User account information may be exposed through SNMP.
- Application and service versions may be disclosed.
- Network configuration details may be accessible.
- Sensitive operational information may be available through MIB objects.
- Exposed information can assist attackers in identifying additional attack vectors.

### Security Risk

Information exposed through SNMP can significantly aid attackers during reconnaissance and vulnerability identification activities.

## Security Impact

The assessment identified multiple security weaknesses that could expose sensitive system and network information through SNMP services.

### Identified Risks

- Default or weak community strings were present.
- Sensitive management information was exposed.
- System and application details were disclosed.
- Insecure SNMP versions may be in use.
- MIB objects provided valuable reconnaissance data.

### Potential Impact

An attacker could gather sensitive information about the target environment, identify vulnerable applications, discover usernames, and use the collected data to support further attacks against the network.

## Remediation

The following recommendations can help improve the security of SNMP services:

- Disable SNMP if it is not required.
- Remove default and weak community strings.
- Use complex community names.
- Restrict SNMP access using firewall rules and access control lists.
- Limit SNMP access to authorized management systems only.
- Upgrade to SNMPv3 for authentication and encryption.
- Monitor SNMP activity and logs regularly.
- Keep network devices and SNMP services updated with the latest security patches.

## Tools Used

- Nmap
- netdiscover
- snmp-check
- snmpwalk
- Kali Linux

## Key Learning

This assessment provided hands-on experience in discovering SNMP services, testing community strings, enumerating MIB objects, and identifying sensitive information exposed through SNMP.

The exercise reinforced the importance of secure SNMP configurations, strong community strings, restricted access controls, and the use of SNMPv3 for secure management communications.

## Disclaimer

This assessment was conducted in a controlled lab environment for educational and ethical security testing purposes only.