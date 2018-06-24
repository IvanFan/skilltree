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





