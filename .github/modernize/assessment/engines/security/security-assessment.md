# Security Assessment Report

**Generated:** 06/22/2026 10:05:16

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 9 |
| CVE Vulnerabilities | 5 |
| CWE Vulnerabilities | 4 |
| Total Rules Assessed | 59 |
| Rules Passed | 55 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 5 |
| optional |  |
| potential | 4 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2023-33170: Microsoft Security Advisory CVE-2023-33170: .NET Security Feature Bypass Vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** packages.config:10

[CVE-2023-33170](https://github.com/advisories/GHSA-25c8-p796-jg6r): Microsoft Security Advisory CVE-2023-33170: .NET Security Feature Bypass Vulnerability

Severity: HIGH

Affected dependencies:
  - Microsoft.AspNet.Identity.Owin:2.1.0 (declared at packages.config:10)

Recommended fix:
  - Upgrade Microsoft.AspNet.Identity.Owin to 2.2.4 or later

### CVE-2022-29117: .NET Denial of Service Vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** packages.config:19, packages.config:16

[CVE-2022-29117](https://github.com/advisories/GHSA-3rq8-h3gj-r5c6): .NET Denial of Service Vulnerability

Severity: HIGH

Affected dependencies:
  - Microsoft.Owin.Security.Cookies:2.1.0 (declared at packages.config:19)
  - Microsoft.Owin:2.1.0 (declared at packages.config:16)

Recommended fix:
  - Upgrade Microsoft.Owin.Security.Cookies to 4.2.2 or later
  - Upgrade Microsoft.Owin to 4.2.2 or later

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** packages.config:27

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Improper Handling of Exceptional Conditions in Newtonsoft.Json

Severity: HIGH

Affected dependencies:
  - Newtonsoft.Json:5.0.6 (declared at packages.config:27)

Recommended fix:
  - Upgrade Newtonsoft.Json to 13.0.1 or later

### CVE-2020-1045: Cookie parsing failure
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** packages.config:16

[CVE-2020-1045](https://github.com/advisories/GHSA-hxrm-9w7p-39cc): Cookie parsing failure

Severity: HIGH

Affected dependencies:
  - Microsoft.Owin:2.1.0 (declared at packages.config:16)

Recommended fix:
  - Upgrade Microsoft.Owin to 4.1.1 or later

### CVE-2021-21252: Regular Expression Denial of Service in jquery-validation
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** packages.config:7

[CVE-2021-21252](https://github.com/advisories/GHSA-jxwx-85vp-gvwm): Regular Expression Denial of Service in jquery-validation

Severity: HIGH

Affected dependencies:
  - jQuery.Validation:1.11.1 (declared at packages.config:7)

Recommended fix:
  - Upgrade jQuery.Validation to 1.19.3 or later

## CWE Findings (Code-Level Vulnerabilities)

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** Controllers/AdministrationController.cs, Controllers/AppointmentController.cs, Controllers/DoctorController.cs, Controllers/RegisteredUsersController.cs, Models/BuisnessLogic.cs, Models/IdentityManager.cs

Multiple HospitalDbContext instances are created as class fields or inline without being wrapped in using statements or try/finally blocks. In RegisteredUsersController, BuisnessLogic, and IdentityManager, DbContext instances are created directly in methods without guaranteed disposal if an exception occurs, risking resource leaks.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** Controllers/HomeController.cs

In HomeController, the DbContext (which implements IDisposable and manages database connection handles) is disposed via a Dispose() method override but is not wrapped in a using or try/finally block within action methods, meaning connection handles may not be released if an exception is thrown before Dispose is called.

### CWE-1057: Data Access Operations Outside of Expected Data Manager Component
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** Controllers/AdministrationController.cs, Controllers/AppointmentController.cs, Controllers/DoctorController.cs, Controllers/HomeController.cs, Controllers/RegisteredUsersController.cs, Models/BuisnessLogic.cs, Models/IdentityManager.cs

Controllers directly instantiate and query HospitalDbContext instead of going through a repository or service layer. AdministrationController, AppointmentController, DoctorController, HomeController, and RegisteredUsersController all perform direct LINQ queries against DbContext fields. BuisnessLogic and IdentityManager also directly access DbContext without an abstraction layer.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** Controllers/AccountController.cs, Controllers/AdministrationController.cs, Controllers/RegisteredUsersController.cs

The application has no logging infrastructure for security-critical events. In AccountController, failed login attempts (line 56: 'Invalid username or password'), password changes (lines 124-149), and registration actions are not logged. In AdministrationController and RegisteredUsersController, role assignment and user management operations that modify security configurations are performed without any audit logging. The application only has a single reference to logging in Startup.Auth.cs which is not related to security events.

