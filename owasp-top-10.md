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



