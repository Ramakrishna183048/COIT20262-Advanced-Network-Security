
# Week 3 – Vulnerability Analysis and Web Application Security

## Task 1 – Vulnerability Analysis using Greenbone/OpenVAS

In this task, I used Greenbone/OpenVAS to perform a vulnerability scan on the MS2 virtual machine.

The MS2 virtual machine had the IP address:

`192.168.56.35`

I created MS2 as a target in Greenbone and created a scan task named **MS2 Scan** using the **OpenVAS Default** scanner and **Full and fast** scan configuration.

The vulnerability scan completed successfully with a maximum severity of **10.0 (High)**.

![MS2 Target](images/Week3-Task1-MS2-Target.png)

![MS2 Scan Complete](images/Week3-Task1-MS2-Scan-Complete.png)

The scan identified multiple High, Medium and Low severity vulnerabilities on the MS2 system.

![Vulnerability Results](images/Week3-Task1-Vulnerability-Results.png)

I examined the **Distributed Ruby (dRuby/DRb) Multiple RCE Vulnerabilities**, which had a severity of **10.0 (High)**. The report showed that this vulnerability could allow unauthorised execution of commands on the vulnerable system. I also reviewed the mitigation information provided by Greenbone.

![dRuby Vulnerability Details](images/Week3-Task1-dRuby-Vulnerability-Details.png)


## Task 2 – SQL Injection

In this task, I used the Damn Vulnerable Web Application (DVWA) running on MS2 to demonstrate an SQL injection vulnerability.

I logged into DVWA and changed the DVWA security level to **Low**. I then opened the **SQL Injection** vulnerability page and entered:

`1' OR '1 = 1`

The SQL injection caused the application to return multiple user records instead of a single user record. This demonstrated how improperly handled user input can manipulate a database query.

![SQL Injection Result](images/Week3-Task2-SQL-Injection-Result.png)


## Task 3 – OWASP Top 10

In this task, I visited the OWASP website and reviewed the current **OWASP Top 10:2025** web application security risks.

I particularly reviewed the top three risks and their example attack scenarios:

1. **A01:2025 – Broken Access Control**
2. **A02:2025 – Security Misconfiguration**
3. **A03:2025 – Software Supply Chain Failures**

This task helped me understand common web application security risks and the importance of secure access control, correct security configuration, and managing software supply-chain risks.

![OWASP Top 10](images/Week3-Task3-OWASP-Top10.png)


## Week 3 Summary

During Week 3, I gained practical experience with vulnerability assessment and web application security. I used Greenbone/OpenVAS to identify vulnerabilities on MS2, demonstrated an SQL injection vulnerability using DVWA, and reviewed the OWASP Top 10 security risks.
