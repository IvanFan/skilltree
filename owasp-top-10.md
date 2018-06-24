# Injection

# 

Injection flaws are very prevalent, particularly in legacy code. Injection vulnerabilities are often found in SQL, LDAP, XPath, or NoSQL queries, OS commands, XML parsers, SMTP headers, expression languages, and ORM queries.

An application is vulnerable to attack when:

* User-supplied data is not validated, filtered, or sanitized by the application.

* Dynamic queries or non-parameterized calls without context- aware escaping are used directly in the interpreter.

* Hostile data is used within object-relational mapping \(ORM\) search parameters to extract additional, sensitive records.

* Hostile data is directly used or concatenated, such that the SQL or command contains both structure and hostile data in dynamic queries, commands, or stored procedures.

  Some of the more common injections are SQL, NoSQL, OS command, Object Relational Mapping \(ORM\), LDAP, and Expression Language \(EL\) or Object Graph Navigation Library \(OGNL\) injection. The concept is identical among all interpreters. Source code review is the best method of detecting if applications are vulnerable to injections, closely followed by thorough automated testing of all parameters, headers, URL, cookies, JSON, SOAP, and XML data inputs.

Preventing injection requires keeping data separate from commands and queries.

* The preferred option is to use a safe API, which avoids the use of the interpreter entirely or provides a parameterized interface, or migrate to use Object Relational Mapping Tools \(ORMs\).Note: Even when parameterized, stored procedures can still introduce SQL injection if PL/SQL or T-SQL concatenates queries and data, or executes hostile data with EXECUTE IMMEDIATE or exec\(\).

* Use positive or "whitelist" server-side input validation. This is not a complete defense as many applications require special characters, such as text areas or APIs for mobile applications.

* For any residual dynamic queries, escape special characters using the specific escape syntax for that interpreter.  
  Note: SQL structure such as table names, column names, and so on cannot be escaped, and thus user-supplied structure names are dangerous. This is a common issue in report-writing software.

* Use LIMIT and other SQL controls within queries to prevent mass disclosure of records in case of SQL injection.

# Broken Authentication

Attackers have access to hundreds of millions of valid username and password combinations for credential stuffing, default administrative account lists, automated brute force, and dictionary attack tools. Session management attacks are well understood, particularly in relation to unexpired session tokens.

Confirmation of the user's identity, authentication, and session management are critical to protect against authentication-related attacks.  
 There may be authentication weaknesses if the application:

* Permits automated attacks such ascredential stuffing, where the attacker has a list of valid usernames and passwords.

* Permits brute force or other automated attacks.

* Permits default, weak, or well-known passwords, such as"Password1" or "admin/admin“.

* Uses weak or ineffective credential recovery and forgot- password processes, such as "knowledge-based answers", which cannot be made safe.

* Uses plain text, encrypted, or weakly hashed passwords \(seeA3:2017-Sensitive Data Exposure\).

* Has missing or ineffective multi-factor authentication.

* Exposes Session IDs in the URL \(e.g., URL rewriting\).

* Does not rotate Session IDs after successful login.

* Does not properly invalidate Session IDs. User sessions or

  authentication tokens \(particularly single sign-on \(SSO\) tokens\)aren’t properly invalidated during logout or a period of inactivity.

How to Prevent

* Where possible, implement multi-factor authentication to prevent automated, credential stuffing, brute force, and stolen credential re-use attacks.

* Do not ship or deploy with any default credentials, particularly for admin users.

* Implement weak-password checks, such as testing new or changed passwords against a list of thetop 10000 worst passwords.

* Align password length, complexity and rotation policies with

  NIST 800-63 B's guidelines in section 5.1.1 for Memorized Secretsor other modern, evidence based password policies.

* Ensure registration, credential recovery, and API pathways are hardened against account enumeration attacks by using the same messages for all outcomes.

* Limit or increasingly delay failed login attempts. Log all failures and alert administrators when credential stuffing, brute force, or other attacks are detected.

* Use a server-side, secure, built-in session manager that generates a new random session ID with high entropy after login. Session IDs should not be in the URL, be securely stored and invalidated after logout, idle, and absolute timeouts.

# Sensitive Data Exposure

Rather than directly attacking crypto, attackers steal keys, execute man-in- the-middle attacks, or steal clear text data off the server, while in transit, orfrom the user’s client, e.g. browser. Amanual attack is generally required. Previously retrieved password databases could be brute forced by Graphics Processing Units \(GPUs\).

Over the last few years, this has been the most common impactful attack. The most common flaw is simply not encrypting sensitive data. When crypto is employed, weak key generation and management, and weak algorithm, protocol and cipher usage is common, particularly for weak password hashing storage techniques. For data in transit, server side weaknesses are mainly easy to detect, but hard for data at rest.

OWASP Top 10 - 2017

The first thing is to determine the protection needs of data in transit and at rest. For example, passwords, credit card numbers, health records, personal information and business secrets require extra protection, particularly if that data falls under privacy laws, e.g. EU's General Data Protection Regulation \(GDPR\), or regulations, e.g. financial data protection such as PCI Data Security Standard \(PCI DSS\). For all such data:

* Is any data transmitted in clear text? This concerns protocols

  such as HTTP, SMTP, and FTP. External internet traffic is especially dangerous. Verify all internal traffic e.g. between load balancers, web servers, or back-end systems.

* Is sensitive data stored in clear text, including backups?

* Are any old or weak cryptographic algorithms used either by default or in older code?

* Are default crypto keys in use, weak crypto keys generated or re-used, or is proper key management or rotation missing?

* Is encryption not enforced, e.g. are any user agent \(browser\) security directives or headers missing?

* Does the user agent \(e.g. app, mail client\) not verify if the received server certificate is valid?

Do the following, at a minimum, and consult the references:

* Classify data processed, stored, or transmitted by an application. Identify which data is sensitive according to privacy laws, regulatory requirements, or business needs.

* Apply controls as per the classification.

* Don’t store sensitive data unnecessarily.Discard it as soon as possible or use PCI DSS compliant tokenization or even truncation. Data that is not retained cannot be stolen.

* Make sure to encrypt all sensitive data at rest.

* Ensure up-to-date and strong standard algorithms, protocols,

  and keys are in place; use proper key management.

* Encrypt all data in transit with secure protocols such as TLS with perfect forward secrecy \(PFS\) ciphers, cipher prioritization by the server, and secure parameters. Enforce encryption using directives like HTTP Strict Transport Security \(HSTS\).

* Disable caching for responses that contain sensitive data.

* Store passwords using strong adaptive and salted hashing functions with a work factor \(delay factor\), such asArgon2,scrypt,bcrypt, orPBKDF2.

* Verify independently the effectiveness of configuration and settings.

# XML External Entities \(XXE\)

Attackers can exploit vulnerable XML processors if they can upload XML or include hostile content in an XML document, exploiting vulnerable code, dependencies or integrations.

Applications and in particular XML-based web services or downstream integrations might be vulnerable to attack if:

* The application accepts XML directly or XML uploads, especially from untrusted sources, or inserts untrusted data into XML documents, which is then parsed by an XML processor.

* Any of the XML processors in the application or SOAP based web services hasdocument type definitions \(DTDs\)enabled. As the exact mechanism for disabling DTD processing varies by processor, it is good practice to consult a reference such as theOWASP Cheat Sheet 'XXE Prevention’.

* If your application uses SAML for identity processing within federated security or single sign on \(SSO\) purposes. SAML uses XML for identity assertions, and may be vulnerable.

* If the application uses SOAP prior to version 1.2, it is likely susceptible to XXE attacks if XML entities are being passed to the SOAP framework.

* Being vulnerable to XXE attacks likely means that the application is vulnerable to denial of service attacks including the Billion Laughs attack.

OWASP Top 10 - 2017

Developer training is essential to identify and mitigate XXE.

Besides that, preventing XXE requires:

* Whenever possible, use less complex data formats such as JSON, and avoiding serialization of sensitive data.

* Patch or upgrade all XML processors and libraries in use by the application or on the underlying operating system. Use dependency checkers. Update SOAP to SOAP 1.2 or higher.

* Disable XML external entity and DTD processing in all XML parsers in the application, as per theOWASP Cheat Sheet 'XXE Prevention'.

* Implement positive \("whitelisting"\) server-side input validation, filtering, or sanitization to prevent hostile data within XML documents, headers, or nodes.

* Verify that XML or XSL file upload functionality validates incoming XML using XSD validation or similar.

* SASTtools can help detect XXE in source code, although manual code review is the best alternative in large, complex applications with many integrations.

  If these controls are not possible, consider using virtual patching, API security gateways, or Web Application Firewalls \(WAFs\) to detect, monitor, and block XXE attacks.

# Broken Access Control

Access control weaknesses are common due to the lack of automated detection, and lack of effective functional testing by application developers.

Access control detection is not typically amenable to automated static or dynamic testing. Manual testing is the best way to detect missing or ineffective access control, including HTTP method \(GET vs PUT, etc\), controller, direct object references, etc.

Access control enforces policy such that users cannot act outside of their intended permissions. Failures typically lead to unauthorized information disclosure, modification or destruction of all data, or performing a business function outside of the limits of the user. Common access control vulnerabilities include:

* Bypassing access control checks by modifying the URL, internal application state, or the HTML page, or simply using a custom API attack tool.

* Allowing the primary key to be changed to another users record, permitting viewing or editing someone else's account.

* Elevation of privilege. Acting as a user without being logged in, or acting as an admin when logged in as a user.

* Metadata manipulation, such as replaying or tampering with a JSON Web Token \(JWT\) access control token or a cookie or hidden field manipulated to elevate privileges, or abusing JWT invalidation

* CORS misconfiguration allows unauthorized API access.

* Force browsing to authenticated pages as an unauthenticated user or to privileged pages as a standard user. Accessing API with missing access controls for POST, PUT and DELETE.

OWASP Top 10 - 2017

Access control is only effective if enforced in trusted server-side code or server-less API, where the attacker cannot modify the access control check or metadata.

* With the exception of public resources, deny by default.

* Implement access control mechanisms once and re-use them throughout the application, including minimizing CORS usage.

* Model access controls should enforce record ownership, rather than accepting that the user can create, read, update, or delete any record.

* Unique application business limit requirements should be enforced by domain models.

* Disable web server directory listing and ensure file metadata \(e.g. .git\) and backup files are not present within web roots.

* Log access control failures, alert admins when appropriate \(e.g. repeated failures\).

* Rate limit API and controller access to minimize the harm from automated attack tooling.

* JWT tokens should be invalidated on the server after logout.

  Developers and QA staff should include functional access control unit and integration tests.

# Security Misconfiguration

Security misconfiguration can happen at any level of an application stack, including the network services, platform, web server, application server, database, frameworks, custom code, and pre-installed virtual machines, containers, or storage. Automated scanners are useful for detecting misconfigurations, use of default accounts or configurations, unnecessary services, legacy options, etc.

OWASP Top 10 - 2017

The application might be vulnerable if the application is:

* Missing appropriate security hardening across any part of the application stack, or improperly configured permissions on cloud services.

* Unnecessary features are enabled or installed \(e.g. unnecessary ports, services, pages, accounts, or privileges\).

* Default accounts and their passwords still enabled and unchanged.

* Error handling reveals stack traces or other overly informative error messages to users.

* For upgraded systems, latest security features are disabled or not configured securely.

* The security settings in the application servers, application frameworks \(e.g. Struts, Spring, ASP.NET\), libraries, databases, etc. not set to secure values.

* The server does not send security headers or directives or they are not set to secure values.

* The software is out of date or vulnerable \(seeA9:2017-Using Components with Known Vulnerabilities\).

  Without a concerted, repeatable application security configuration process, systems are at a higher risk.

Secure installation processes should be implemented, including:

* A repeatable hardening process that makes it fast and easy to

  deploy another environment that is properly locked down. Development, QA, and production environments should all be configured identically, with different credentials used in each environment. This process should be automated to minimize the effort required to setup a new secure environment.

* A minimal platform without any unnecessary features, components, documentation, and samples. Remove or do not install unused features and frameworks.

* A task to review and update the configurations appropriate to all security notes, updates and patches as part of the patch management process \(seeA9:2017-Using Components with Known Vulnerabilities\). In particular, review cloud storage permissions \(e.g. S3 bucket permissions\).

* A segmented application architecture that provides effective, secure separation between components or tenants, with segmentation, containerization, or cloud security groups.

* Sending security directives to clients, e.g.Security Headers.

* An automated process to verify the effectiveness of the configurations and settings in all environments.

# Cross-Site Scripting \(XSS\)

There are three forms of XSS, usually targeting users' browsers:

Reflected XSS:The application or API includes unvalidated and unescaped user input as part of HTML output. A successful attack can allow the attacker to execute arbitrary HTML andJavaScript in the victim’s browser. Typically the user will need tointeract with some malicious link that points to an attacker- controlled page, such as malicious watering hole websites, advertisements, or similar.

Stored XSS:The application or API stores unsanitized user input that is viewed at a later time by another user or an administrator. Stored XSS is often considered a high or critical risk.

DOM XSS:JavaScript frameworks, single-page applications, and APIs that dynamically include attacker-controllable data to a page are vulnerable to DOM XSS. Ideally, the application would not send attacker-controllable data to unsafe JavaScript APIs.

Typical XSS attacks include session stealing, account takeover, MFA bypass, DOM node replacement or defacement \(such as trojan login panels\), attacks against the user's browser such as malicious software downloads, key logging, and other client-side attacks.

Preventing XSS requires separation of untrusted data from active browser content. This can be achieved by:

* Using frameworks that automatically escape XSS by design, such as the latest Ruby on Rails, React JS. Learn the limitations of each framework's XSS protection and appropriately handle the use cases which are not covered.

* Escaping untrusted HTTP request data based on the context in the HTML output \(body, attribute, JavaScript, CSS, or URL\) will resolve Reflected and Stored XSS vulnerabilities. TheOWASP Cheat Sheet 'XSS Prevention'has details on the required data escaping techniques.

* Applying context-sensitive encoding when modifying the browser document on the client side acts against DOM XSS. When this cannot be avoided, similar context sensitive escaping techniques can be applied to browser APIs as described in theOWASP Cheat Sheet 'DOM based XSS Prevention'.

* Enabling aContent Security Policy \(CSP\)is a defense-in-depth mitigating control against XSS. It is effective if no other vulnerabilities exist that would allow placing malicious code via local file includes \(e.g. path traversal overwrites or vulnerable libraries from permitted content delivery networks\).



## Insecure Deserialization



Exploitation of deserialization is somewhat difficult, as off the shelf exploits rarely work without changes or tweaks to the underlying exploit code.

OWASP Top 10 - 2017

Applications and APIs will be vulnerable if they deserialize hostile or tampered objects supplied by an attacker.

This can result in two primary types of attacks:

* Object and data structure related attacks where the attacker

  modifies application logic or achieves arbitrary remote code execution if there are classes available to the application that can change behavior during or after deserialization.

* Typical data tampering attacks, such as access-control-related attacks, where existing data structures are used but the content is changed.

  Serialization may be used in applications for:

* Remote- and inter-process communication \(RPC/IPC\)

* Wire protocols, web services, message brokers

* Caching/Persistence

* Databases, cache servers, file systems

* HTTP cookies, HTML form parameters, API authentication tokens



The only safe architectural pattern is not to accept serialized objects from untrusted sources or to use serialization mediums that only permit primitive data types.  
 If that is not possible, consider one of more of the following:

* Implementing integrity checks such as digital signatures on any serialized objects to prevent hostile object creation or data tampering.

* Enforcing strict type constraints during deserialization before object creation as the code typically expects a definable set of classes. Bypasses to this technique have been demonstrated, so reliance solely on this is not advisable.

* Isolating and running code that deserializes in low privilege environments when possible.

* Logging deserialization exceptions and failures, such as where the incoming type is not the expected type, or the deserialization throws exceptions.

* Restricting or monitoring incoming and outgoing network connectivity from containers or servers that deserialize.

* Monitoring deserialization, alerting if a user deserializes constantly.



