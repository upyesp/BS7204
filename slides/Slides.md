---
marp: true
theme: custom-default
class: invert
transition: fade
paginate: true
---

# Learning Café Penetration Test

## University of Winchester
![UoW Logo](./img/uow.png)
Module: BS7204 Network Security and Penetration Testing
Lecturer: Rhys Lockley
Student: Peter Torkington, ID: 1902008

---

## Presentation Overview

1. Testing Methodology - NIST SP 800-115
2. Evidence of Performed Penetration Test
3. Countermeasures and Secure Design
4. Conclusion
5. References
6. Questions

---

## Penetration Testing Methodology Used: NIST SP 800-115

### 1. Planning

Define objectives, scope, and rules of engagement.

### 2. Discovery

Active and passive reconnaissance.

### 3. Attack

Exploiting vulnerabilities to validate findings.

### 4. Reporting

Categorising vulnerabilities and proposing mitigations.

---

<script src="https://asciinema.org/a/45UBTj01G7Yf7Sz5dDxgh2iiN.js" id="asciicast-45UBTj01G7Yf7Sz5dDxgh2iiN" 
  async="true" 
  data-preload="true" 
  data-poster='npt:0:01' 
></script>

---

## Practical Evidence: Discovery Phase - Network Mapping

Find the IP of Kali VM & the network address range: `ip addr`.

![IP of Kali and subnet](./img/Network_Address_Range_and_Kali_IP.png)

Discover devices on the network: `nmap -sn 10.0.2.1/24`

![network devices discovered](./img/Network_Devices_Discovered.png)

---

## Practical Evidence: Discovery Phase - Vulnerabilities (OpenVAS)

OpenVAS scan results, summary & detailed:

![OpenVAS Results Summary](./img/OpenVAS_Results_By_Severity_Class_Pie.png)

![OpenVAS Results Detail](./img/OpenVAS_Results_By_Severity_Table.png)

---

## Practical Evidence: Discovery Phase - Vulnerabilities (nmap)

`nmap` ports & services scan results:

![ports & services scan](./img/M2nmap.png)

---

## Practical Evidence: Attack Phase - vsftpd v2.3.4 Backdoor (root)

Root access was obtained by exploiting a backdoor in vsftpd, version '2.3.4'.

![vsftpd v2.3.4](./img/M2ftp.png)

---

## Practical Evidence: Attack Phase - Samba (root)

Root access was obtained by exploiting Samba. Version '3.0.20-Debian' was identified, which is vulnerable to the username map script command execution exploit.

![Samba Exploit](./img/M2samba.png)

---

## Practical Evidence: Attack Phase - Unreal IRCd v3.2.8.1 (root)

Root access was obtained by exploiting a backdoor in unrealIRCd 'v3.2.8.1'.

![Unreal IRCd Exploit](./img/M2unrealIRC.png)

---

## Practical Evidence: Attack Phase - VNC Password Scanner (root)

No tools required.  Default credentials wre used, 'root:toor', failed.  Tried 'root:null' (no password), success.  Full access to all databases and tables.

```bash
mysql -u root -p -h 10.0.2.4 --ssl=off
```

![MySQL](./img/M2mysql.png)

---

## Practical Evidence: Attack Phase - MySQL (root)

Root access was obtained by exploiting a poorly configured VNC server.  A password scanner module in Metasploite was used, attempting multiple passwords.  Access was gained with a password of... 'password'.

![VNC](./img/M2vnc.png)

---

## Practical Evidence: Attack Phase - Telnet (root)

Root access was obtained by exploiting Telnet default credentials.

```bash
telnet 10.0.2.4 # login: msfadmin:msfadmin
# then switch to root...
sudo su -
```

![Telnet](./img/M2telnet.png)

---

## Practical Evidence: Attack Phase - ProFTPD v1.3.5

Access was obtained by remote code execution (RCE) vulnerability in ProFTPD 'v1.3.5'.

![Telnet](./img/M3proftpd.png)

---

## Tools Used
<div class="columns">
<div>

### Network 

- `nmap` for network mapping
- OpenVAS for vulnerability scanning

</div>
<div>

### Exploits

- Metasploit Framework v6

</div>
</div>

---

## Practical Evidence: Findings

- Discovered devices: Metasploitable2, Metasploitable3, Debian 
- Total vulnerabilities (OpennVAS):
  - High severity: 30.
  - Medium severity: 49.
---

## Practical Evidence: Exploits

### Metasploitable2

- FTP (ProFTPD): Remote Code Execution (CVE-2015-3306)
- Telnet: Default credentials used to gain access
- Samba share: Uploaded malicious web shell

### Metasploitable3

- Apache (mod_cgi): Exploited Shellshock vulnerability (CVE-2014-6271)
- CUPS: Remote Code Execution

---

## Key Findings

### Critical Vulnerabilities

- FTP and Telnet on Metasploitable2
- Apache and CUPS on Metasploitable3

### Moderate Vulnerabilities

- Samba shares
- MySQL misconfigurations

### Impact

- Unauthorised access to sensitive systems and data
- Potential for university-wide compromise

---

## Secure Design Recommendations

### Network Security

1. Implement segmentation between public and private networks.
1. Remove public access to Wi-fi, or create is as a stand-alone network routing to an new, independent  ISP.

### Authentication

1. Extend use of multi-factor authentication (MFA).
1. Enforce password complexity and regular updates.

### Application Security

1. Regular patching of software and systems.
1. Conduct code reviews and dynamic testing.

---

## Secure Infrastructure

### heading

1. kjhkjhkj
1. jhgsdfgjhgasd
1. jhgsdfgsdg

---

## Countermeasure Justifications

### Addressed Vulnerabilities:

- **FTP:** Disable unencrypted FTP services.
- **Apache & CUPS:** Regular updates and secure configurations.
- **Telnet:** Replace with SSH for secure remote access.

### Infrastructure Enhancements:

- Harden BYOD policies.
- Introduce VLANs for network segmentation.

---

## Conclusion

### Key Outcomes

- Identified critical vulnerabilities in network and application layers.
- Proposed actionable countermeasures for improvement.

### Next Steps

- Implement secure design recommendations.
- Regularly review and update security practices.

---

## References

Computer Misuse Act (1990). Statute Law Database Available at: [https://www.legislation.gov.uk/ukpga/1990/18/contents](https://www.legislation.gov.uk/ukpga/1990/18/contents).

NIST (2014) NVD - cve-2014-6271. Available at: [https://nvd.nist.gov/vuln/detail/cve-2014-6271](https://nvd.nist.gov/vuln/detail/cve-2014-6271).

NIST (2015) NVD - CVE-2015-3306. Available at: [https://nvd.nist.gov/vuln/detail/CVE-2015-3306](https://nvd.nist.gov/vuln/detail/CVE-2015-3306).

Data Protection Act (2018). King’s Printer of Acts of Parliament Available at: [https://www.legislation.gov.uk/ukpga/2018/12/contents/enacted](https://www.legislation.gov.uk/ukpga/2018/12/contents/enacted).

Scarfone, K., Souppaya, M., Cody, A., and Orebaugh, A. (2021) NIST SP 800-115. NIST SP 800-115. Available at: [https://www.nist.gov/privacy-framework/nist-sp-800-115text](https://www.nist.gov/privacy-framework/nist-sp-800-115).

---

## Thank You

### Questions?

---

## Slide 1

- Item 1
- Item 2
- Item 3

![Image](./img/test_animation.gif)

---

## Slide 2

![Image](https://picsum.photos/800/600)

---

## Slide 3

> This is a quote.

---

## Slide 4

| Column 1 | Column 2 |
| -------- | -------- |
| Item 1   | Item 2   |
| Item 3   | Item 4   |

---

![bg opacity](https://picsum.photos/800/600?image=53)
## Slide 5

<div class="columns">
<div>

## Left

- 1
- 2

</div>
<div>

## Right

- 3
- 4

</div>
</div>

---

## Slide 6

<i class="fa-brands fa-twitter"></i> Twitter: 
<i class="fa-brands fa-mastodon"></i> Mastodon: 
<i class="fa-brands fa-linkedin"></i> LinkedIn: 
<i class="fa fa-window-maximize"></i> Blog: 
<i class="fa-brands fa-github"></i> GitHub: 

---

# <!--fit--> Large Text

---

<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
  mermaid.initialize({ startOnLoad: true });
</script>

# Mermaid

<div class="mermaid">
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
</div>

---

## Scope of the Learning Café Network & Systems

- Network infrastructure (wired & wireless)
- Virtual Learning Environment (VLE)
- Email services and academic databases
- Online ordering system
- Workstations and printers

---

### Constraints

- Legal compliance (Computer Misuse Act 1990, DPA 2018)
- Ethical guidelines (NIST SP 800-115)
- Virtual environment (Metasploitable2 & Metasploitable3)

---

## Learning Café Vulnerabilities (1 of 2)

### Network Infrastructure

- Weak segmentation between public and private (UoW) networks
- Risks from public Wi-Fi access points

### Authentication & Access Control

- Susceptibility to phishing
- Insecure BYOD connections

---

## Learning Café Vulnerabilities (2 of 2)

### Web-Based Applications

- SQL Injection & Cross-Site Scripting (XSS)
- Apache

### Emerging Threats

- AI-driven attacks
- Ransomware targeting educational institutions.

---