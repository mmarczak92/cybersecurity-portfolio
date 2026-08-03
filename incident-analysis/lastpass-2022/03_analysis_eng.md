# LastPass 2022 Incident Analysis

## Sources
1. Incident Report | https://blog.lastpass.com/posts/security-incident-update-recommended-actions
2. MITRE D3FEND Matrix | https://d3fend.mitre.org/
3. MITRE ATT&CK Matrix | https://attack.mitre.org/
4. Cyber Kill Chain | https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html

## 1. Incident Name

No official incident name was assigned. Throughout its reports, LastPass refers to the events simply as **Incident 1** and **Incident 2**.

---

## 2. Organization and Year

**LastPass** – a password management service provider, owned by **GoTo** at the time of the incident.

Two security incidents occurred in 2022:

1. Initial attacker activity between **August 8 and August 12, 2022**.
2. A second incident consisting of a series of reconnaissance, enumeration, and data exfiltration activities targeting cloud storage resources between **August 12 and October 26, 2022**, using information obtained during Incident 1.

---

## 3. Threat Actor Profile

Due to the lack of conclusive evidence:

- no contact from the attacker(s),
- no ransom demands,
- no credible evidence that the stolen data was offered for sale,

the threat actor's profile and motivation cannot be reliably determined.

### 3.1 Personal Assessment

Based on the available information:

- a multi-month operation,
- information stolen during the first incident was leveraged to conduct the second attack,
- reconnaissance, enumeration, and data exfiltration activities,
- a multi-stage attack campaign,

the most likely scenarios include advanced cybercriminal activity, corporate espionage, or an Advanced Persistent Threat (APT).

However, the available evidence does not allow the attacker to be definitively attributed to any of these categories, nor does it allow the others to be ruled out.

---

## 4. Initial Access Vector

### Incident 1

**Unknown.**

The report does not disclose the initial access vector.

It only confirms that the attacker gained access to the development environment by compromising a software engineer's account.

### Incident 2

The initial access vector was the exploitation of a vulnerability in third-party media software installed on the personal computer of one of four DevOps engineers, enabling **Remote Code Execution (RCE).**

---

## 5. Objective

### Incident 1

Access and exfiltration of LastPass technical documentation and source code repositories.

Some of the source code repositories contained:

- plaintext credentials,
- digital certificates used within development environments,
- encrypted credentials used in production environments, including backup operations.

The encrypted credentials required a separate decryption key, which was not accessible to either the compromised engineer or the attacker during the first incident.

### Incident 2

Access to and exfiltration of production backups, additional cloud-based storage resources, and related critical database backups by capturing the **Master Password** and exporting the contents of the LastPass Corporate Vault and shared folders containing **Secure Notes** with AWS access keys and decryption keys.

These keys were required to access LastPass production backups stored in **AWS S3**.

---

## 6. CIA Triad Impact

### Incidents 1 & 2

**Confidentiality**

- **Compromised**
- Unauthorized access to and exfiltration of technical documentation, source code repositories, production backups, and other sensitive information.

**Integrity**

- **Not compromised**
- The report contains no evidence that systems or data were modified or tampered with.

**Availability**

- **Not compromised**
- The report does not indicate any service disruption or loss of system availability caused by the attacker.

---

## 7. Cyber Kill Chain

### Incident 1

- **Reconnaissance** – No data available.
- **Weaponization** – No data available.
- **Delivery** – No data available.
- **Exploitation** – No data available.
- **Installation** – No data available.
- **Command & Control** – The attacker established and maintained access to the development environment using the compromised software engineer's account, concealed activity through a third-party VPN, and tampered with the EDR agent to evade detection.
- **Actions on Objectives** – Access to and exfiltration of technical documentation and portions of LastPass source code repositories.

### Incident 2

- **Reconnaissance** – Information obtained during Incident 1 and data exposed through a third-party breach were used to identify one of four DevOps engineers with access to resources required to access production backups.
- **Weaponization** – No data available.
- **Delivery** – The attacker targeted the engineer's personal computer by exploiting a vulnerability in third-party media software.
- **Exploitation** – Exploitation of the vulnerability resulting in Remote Code Execution (RCE).
- **Installation** – Deployment of keylogger malware.
- **Command & Control** – Access to the LastPass Corporate Vault using the captured **Master Password**, enabling continued activity through compromised credentials.
- **Actions on Objectives** – Export of the Corporate Vault and shared folders, acquisition of AWS access keys and decryption keys, access to production backups, and data exfiltration.

---

## 8. What Went Wrong on the Defender's Side?

- The initial attack vector was never identified (Incident 1).
- Monitoring and alerting failed to detect anomalous activity early enough (Incident 1).
- Plaintext credentials were stored within source code repositories.
- A DevOps engineer was able to access decryption keys from a personal device used for remote work (Incident 2).

---

## 9. Where Could the Incident Have Been Interrupted?

### Incident 1

- Following the compromise of the engineer's laptop through earlier workstation compromise detection.
- During attempts to tamper with the EDR agent by detecting attempts to disable or bypass security controls.
- During access to the development environment through detection of anomalous VPN usage, behavioral analytics, Impossible Travel detection, or Device Trust verification.
- Prior to source code exfiltration through Data Loss Prevention (DLP) controls and monitoring of large outbound data transfers.

### Incident 2

- During target selection by limiting the amount of information exposed after the first incident and through third-party data breaches.
- On the engineer's workstation by patching the vulnerable third-party media software.
- Immediately after Remote Code Execution (RCE) through effective EDR/XDR detection.
- Following keylogger deployment through malware detection.
- During Master Password usage by detecting anomalous access to the Corporate Vault.
- During Corporate Vault export by generating alerts for abnormal export activity.
- During access to AWS S3 resources. Although AWS GuardDuty eventually generated alerts, earlier detection could have prevented data exfiltration.

---

## 10. Three Security Recommendations

1. Restrict privileged administrative activities and access to critical resources from personal devices.

2. Strengthen monitoring of privileged workstations through tamper-resistant EDR/XDR solutions.

3. Eliminate the storage of plaintext credentials within source code repositories.
