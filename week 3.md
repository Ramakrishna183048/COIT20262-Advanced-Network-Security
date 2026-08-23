# Week 3 – Vulnerability Analysis and Web Application Security

## Task 1 – Vulnerability Analysis using Greenbone/OpenVAS

### Objective
The objective of this task was to use Greenbone/OpenVAS to scan the MS2 virtual machine, identify security vulnerabilities, examine their severity, and review possible mitigation methods.

### Procedure
I used the MS2 virtual machine with the IP address:

`192.168.56.35`

I created MS2 as a target in Greenbone and created a scan task named **MS2 Scan**. The scan used the **OpenVAS Default** scanner with the **Full and fast** scan configuration.

![MS2 Target](images/Week3-Task1-MS2-Target.png)

### Result
The vulnerability scan completed successfully and reported a maximum severity of **10.0 (High)**.

![MS2 Scan Complete](images/Week3-Task1-MS2-Scan-Complete.png)

The scan identified multiple High, Medium and Low severity vulnerabilities.

![Vulnerability Results](images/Week3-Task1-Vulnerability-Results.png)

I examined the **Distributed Ruby (dRuby/DRb) Multiple RCE Vulnerabilities**, which had a severity of **10.0 (High)**. I also reviewed the impact and mitigation information provided in the Greenbone report.

![dRuby Vulnerability Details](images/Week3-Task1-dRuby-Vulnerability-Details.png)


## Task 2 – SQL Injection

### Objective
The objective of this task was to demonstrate an SQL injection vulnerability using the Damn Vulnerable Web Application (DVWA) and observe how malicious input can affect a database query.

### Procedure
I started the MS2 virtual machine and accessed DVWA. After logging in, I changed the **DVWA Security** level to **Low**.

I opened the **SQL Injection** page and entered:

`1' OR '1 = 1`

### Result
The application returned multiple user records instead of a single record. This demonstrated how an SQL injection can manipulate a database query when user input is not handled securely.

![SQL Injection Result](images/Week3-Task2-SQL-Injection-Result.png)


## Task 3 – OWASP Top 10

### Objective
The objective of this task was to review the current OWASP Top 10 web application security risks and, in particular, study the **Example Attack Scenarios for the top three risks**.

### OWASP Top 10:2025

1. [A01:2025 - Broken Access Control](https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/)
2. [A02:2025 - Security Misconfiguration](https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/)
3. [A03:2025 - Software Supply Chain Failures](https://owasp.org/Top10/2025/A03_2025-Software_Supply_Chain_Failures/)
4. [A04:2025 - Cryptographic Failures](https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures/)
5. [A05:2025 - Injection](https://owasp.org/Top10/2025/A05_2025-Injection/)
6. [A06:2025 - Insecure Design](https://owasp.org/Top10/2025/A06_2025-Insecure_Design/)
7. [A07:2025 - Authentication Failures](https://owasp.org/Top10/2025/A07_2025-Authentication_Failures/)
8. [A08:2025 - Software or Data Integrity Failures](https://owasp.org/Top10/2025/A08_2025-Software_or_Data_Integrity_Failures/)
9. [A09:2025 - Security Logging and Alerting Failures](https://owasp.org/Top10/2025/A09_2025-Security_Logging_and_Alerting_Failures/)
10. [A10:2025 - Mishandling of Exceptional Conditions](https://owasp.org/Top10/2025/A10_2025-Mishandling_of_Exceptional_Conditions/)

### Top Three Risks Reviewed

#### 1. A01:2025 – Broken Access Control
I reviewed the example attack scenarios for Broken Access Control. This risk occurs when access restrictions are not correctly enforced, which may allow a user to access resources or perform actions that they should not be authorised to access.

#### 2. A02:2025 – Security Misconfiguration
I reviewed the example attack scenarios for Security Misconfiguration. This risk can occur when applications, servers, frameworks, or security settings are incorrectly configured, potentially exposing sensitive functionality or information to attackers.

#### 3. A03:2025 – Software Supply Chain Failures
I reviewed the example attack scenarios for Software Supply Chain Failures. This risk relates to weaknesses in software dependencies, build processes, distribution systems, or third-party components that can affect the security of an application.

### Result
I reviewed the OWASP Top 10:2025 and studied the example attack scenarios for the top three risks: Broken Access Control, Security Misconfiguration, and Software Supply Chain Failures. This helped me understand how these security weaknesses may affect web applications.


## Week 3 Summary

During Week 3, I gained practical experience in vulnerability assessment and web application security. I used Greenbone/OpenVAS to scan MS2 and analyse detected vulnerabilities, demonstrated SQL injection using DVWA, and reviewed the OWASP Top 10 web application security risks.
