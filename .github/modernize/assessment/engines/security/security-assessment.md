# Security Assessment Report

**Generated:** 2026-06-22T07:10:00Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 10 |
| CVE Vulnerabilities | 5 |
| CWE Vulnerabilities | 5 |
| Total Rules Assessed | 59 |
| Rules Passed | 54 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 5 |
| optional | 2 |
| potential | 3 |

### By Category

| Category | Count |
|----------|-------|
| CVE | 5 |
| Credentials & Secrets | 3 |
| Code Quality | 2 |

---

## CVE Findings (Dependency Vulnerabilities)

### CVE-2021-21252: Regular Expression Denial of Service in jquery-validation
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** packages.config:7

[CVE-2021-21252](https://github.com/advisories/GHSA-jxwx-85vp-gvwm): Regular Expression Denial of Service in jquery-validation

Severity: HIGH

Affected dependencies:
  - jQuery.Validation:1.11.1 (declared at packages.config:7) — vulnerable range: < 1.19.3

Recommended fix:
  - Upgrade jQuery.Validation to 1.19.3 or later

---

### CVE-2023-33170: Microsoft Security Advisory CVE-2023-33170: .NET Security Feature Bypass Vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** packages.config:10

[CVE-2023-33170](https://github.com/advisories/GHSA-25c8-p796-jg6r): .NET Security Feature Bypass Vulnerability

Severity: HIGH

Affected dependencies:
  - Microsoft.AspNet.Identity.Owin:2.1.0 (declared at packages.config:10) — vulnerable range: < 2.2.4

Recommended fix:
  - Upgrade Microsoft.AspNet.Identity.Owin to 2.2.4 or later

---

### CVE-2022-29117: .NET Denial of Service Vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** packages.config:16, packages.config:19

[CVE-2022-29117](https://github.com/advisories/GHSA-3rq8-h3gj-r5c6): .NET Denial of Service Vulnerability

Severity: HIGH

Affected dependencies:
  - Microsoft.Owin:2.1.0 (declared at packages.config:16) — vulnerable range: < 4.2.2
  - Microsoft.Owin.Security.Cookies:2.1.0 (declared at packages.config:19) — vulnerable range: < 4.2.2

Recommended fix:
  - Upgrade Microsoft.Owin to 4.2.2 or later
  - Upgrade Microsoft.Owin.Security.Cookies to 4.2.2 or later

---

### CVE-2020-1045: Cookie parsing failure
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** packages.config:16

[CVE-2020-1045](https://github.com/advisories/GHSA-hxrm-9w7p-39cc): Cookie parsing failure in Microsoft.Owin

Severity: HIGH

Affected dependencies:
  - Microsoft.Owin:2.1.0 (declared at packages.config:16) — vulnerable range: < 4.1.1

Recommended fix:
  - Upgrade Microsoft.Owin to 4.1.1 or later

---

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** packages.config:27

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Improper Handling of Exceptional Conditions in Newtonsoft.Json

Severity: HIGH

Affected dependencies:
  - Newtonsoft.Json:5.0.6 (declared at packages.config:27) — vulnerable range: < 13.0.1

Recommended fix:
  - Upgrade Newtonsoft.Json to 13.0.1 or later

---

## CWE Findings (Code-Level Vulnerabilities)

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** Migrations/Configuration.cs

In Migrations/Configuration.cs, line 51, the seed method hard-codes the admin user password: `idManager.CreateUser(newUser, "12345678")`. This default admin credential is embedded directly in source code and will be seeded into the database on initialization.

---

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** Controllers/AccountController.cs

No logging framework (ILogger, NLog, log4net, Serilog, EventLog, etc.) is used anywhere in the application. Security-critical events such as failed login attempts (AccountController.cs line 56: "Invalid username or password.") and authentication failures are handled via ModelState errors only, with no logging to any audit trail or log sink.

---

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** Migrations/Configuration.cs

In Migrations/Configuration.cs, line 51, the seed method embeds the admin username ("Admin") and password ("12345678") as hard-coded credentials: `idManager.CreateUser(newUser, "12345678")`. These credentials are committed to source control and seeded into the database on startup.

---

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** Models/BuisnessLogic.cs

In BuisnessLogic.cs, HospitalDbContext instances are created locally (e.g., lines 16, 29, 41, 62) without a using statement or explicit Dispose() call. The DbContext (which holds database connections) is never released, causing resource leaks.

---

### CWE-1057: Data Access Operations Outside of Expected Data Manager Component
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** Models/BuisnessLogic.cs

BuisnessLogic.cs (a business logic class) directly instantiates HospitalDbContext (lines 16, 29, 41, 62) to perform database operations, bypassing the controller-managed DbContext lifecycle. Data access in the business logic layer is performed outside the expected data manager (controller-level DbContext) component.
