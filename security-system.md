There are 5 capability streams we need to take care of:

1. Defensive Network
2. Application Compliance
3. Auth & Access control
4. Event management
5. Data Protection

For each stream, there are 4 levels:

Security Framework

|  | Security policy and standard | Risk management | ISMS | GRC |
| :--- | :--- | :--- | :--- | :--- |
|  | level 1 | level 2 | level 3 | level 4 |
| Defensive Network | Next generation firewall | content security | network sandbox | Application security |
| Endpoint & application compliance | Next generation antivirus | patch and vulnerability management | secure operation environment | Secure Devops |
| auth and access control | multi-factor auth | identity based networking | privileged account management | identity access management and single sing on |
| event management | log aggregation | SIEM | Security analysis | Security orchestration |
| data protection | transfer and recovery | Encryption and Data anonymization | Data Loss Prevention | User and Data analysis |

Tips:

Next generation firewall and web application firewall:

Next generation firewall has all features of the existing firewalls and also contains other features like IPS, DPI \(Deep packet inspection\), ssl/ssh, anti virus and etc

web application firewall: designed for web server including http/https and it focus on the application layer by checking the request data and business logic rule

**Some recommended solutions:**

Next generation firewall & IPS （Intrusion prevent system）

Web security solution

web application firewall

next generation anti-virus

formal vulnerability management program

secure operating environment \(SOE\)

multi-factor auth

log aggregation and SIEM \(Security incident and event management\)

secure file transfer

**Points:**

DMZ, setup firewall between different layers

IPS

web-security solution to reduce the management complexity

Consider of single point of failure

next generation anti virus for all endpoints

setup SOE as a standard

regular check of vulnerability and patch management

MFA

centralise the logging system

SIEM to check all logs in real time

backup

**Explain:**

content security controls for outbound web \(http/https\) e.g. email content

network sandbox\(0-day malware protection\) to protect 0-day malware attack

WAF for inbound web \(http/https\) data flows

---

Security devops: secure software development process e.g. security review during the development

formal application security testing

---

MFA

identity-based network access control: enforce authentication authorisation at the point of accessing the network. network access control from internal network

Privilege Access control solution: minimising admin privileges

Single sign on: manage credentials within the organisation

---

secure file transfer solution: to send sensitive files securely over external public network

data encryption on laptops endpoints

HSM based encryption to handle database level encryption

DLP \(**data loss prevention**\) solution: should consider both data in motion and data at rest along with covering all channels e.g. email internal removal devices

DRM: digital rights management solution : is a set of access control technologies for restricting the use of proprietary hardware and copyrighted works.









Extra:

HSM

# 三级密钥体制示意图：

![](http://my.csdn.net/uploads/201208/08/1344427452_3046.png)



　　1. 主密钥用于加密密钥交换密钥和数据密钥作本地存储；



　　2. 密钥交换密钥用于加密数据密钥作网络传输；



　　3. 数据密钥用于对数据进行加解密。

