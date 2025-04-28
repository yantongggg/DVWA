# SQL Injection Exploitation – DVWA

## Overview
This section demonstrates the exploitation of SQL Injection (SQLi) vulnerabilities on the Damn Vulnerable Web Application (DVWA) at high security level.

The goal was to perform SQL Injection attacks to bypass authentication, enumerate the database, and extract sensitive information such as user credentials.

## Tools Used
- Burp Suite (manual interception and payload injection)
- SQLMap (automated SQLi exploitation)

## Techniques Applied
- Authentication Bypass (`' OR '1'='1`)
- Database Enumeration (`UNION SELECT database(), user()`)
- Table and Column Enumeration
- Password Hash Dumping and Cracking

## Screenshots
Screenshots of the exploitation process are provided in the `/screenshots` folder.

## Key Commands
Refer to `commands.txt` for full payloads and SQLMap commands used.

---
