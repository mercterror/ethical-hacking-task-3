# Penetration Test Report and Retest

## 1. Executive Summary

This assessment evaluates selected security controls in an OWASP Juice Shop application running in an isolated local Docker environment.

The assessment covered authentication, access control, input handling and HTTP security headers.

The tests documented in Task 3 did not confirm an exploitable vulnerability. However, the captured HTTP response contained a wildcard CORS header and did not include several commonly recommended security headers.

These observations are treated as potential configuration concerns rather than confirmed vulnerabilities.

The assessment recommends reviewing CORS configuration and introducing appropriate browser security headers. Remediation and retesting results will be recorded separately.

## 2. Scope and Authorization

- Application: OWASP Juice Shop
- Environment: Local Docker lab
- URL: http://127.0.0.1:3000
- Testing type: Authorized educational security assessment
- Scope: Authentication, access control, input handling and HTTP response headers

No external or production systems were tested.

## 3. Technical Methodology

The assessment used the following process:

1. Review the application and identify the testing scope.
2. Inspect login and registration behavior.
3. Test access to profile and basket routes while logged out.
4. Compare basket contents between two test accounts.
5. Test normal, unknown and special-character search inputs.
6. Capture HTTP response headers using cURL.
7. Review observations and assign preliminary risk classifications.
8. Propose remediation and perform retesting where changes are applied.

## 4. Findings and Risk Assessment

### F-01: Wildcard CORS Response Header

- Category: Security configuration
- Preliminary severity: Low – requires validation
- Evidence: Task 3 evidence/security-headers/response-headers.txt
- Observation: Access-Control-Allow-Origin: * was present.
- Potential impact: Depending on the resources exposed and credential configuration, permissive cross-origin access may allow other origins to read application responses.
- Root cause: The application or server configuration returns a wildcard allowed origin.
- Recommendation: Restrict cross-origin access to explicitly trusted origins where required. Review sensitive endpoints and credential handling.
- Status: Potential concern; exploitability not established.

### F-02: Content Security Policy Not Observed

- Category: Browser security configuration
- Preliminary severity: Informational
- Evidence: Task 3 evidence/security-headers/response-headers.txt
- Observation: Content-Security-Policy was not present in the captured response.
- Potential impact: The browser may have fewer restrictions against certain classes of content injection.
- Recommendation: Design and test an application-compatible Content Security Policy.
- Status: Hardening recommendation; no exploit confirmed.

### F-03: Referrer Policy Not Observed

- Category: Browser security configuration
- Preliminary severity: Informational
- Evidence: Task 3 evidence/security-headers/response-headers.txt
- Observation: Referrer-Policy was not present.
- Potential impact: Browser referrer information may be disclosed more broadly than intended, depending on browser defaults and navigation.
- Recommendation: Configure an appropriate Referrer-Policy, such as strict-origin-when-cross-origin.
- Status: Hardening recommendation.

### F-04: HSTS Not Observed

- Category: Transport security configuration
- Preliminary severity: Informational for the local HTTP lab
- Evidence: Task 3 evidence/security-headers/response-headers.txt
- Observation: Strict-Transport-Security was not present.
- Recommendation: Enable HSTS for the production HTTPS deployment after confirming HTTPS works correctly.
- Status: Not applicable as a required control for the current local HTTP-only test.

## 5. Other Test Results

### Authentication
Successful login and registration were observed. An invalid-login test was performed, but its exact response was not recorded. No authentication vulnerability was confirmed.

### Access Control
- Logged-out profile route: No protected profile information displayed.
- Logged-out basket route: Empty basket displayed.
- Two-account basket comparison: Separate baskets observed.

No unauthorized data exposure was observed in these tests.

### Input Handling
- apple: Normal search results appeared.
- test123xyz: No results appeared.
- test & sample: No results appeared.

No injection vulnerability was confirmed by these basic tests.

## 6. Remediation Plan

1. Review and restrict wildcard CORS configuration where cross-origin access is not required.
2. Add a suitable Content-Security-Policy after checking application compatibility.
3. Add an appropriate Referrer-Policy.
4. Enable HSTS in the HTTPS production deployment.
5. Retest application behavior and response headers after changes.

## 7. Retest Results

Retesting must be based on actual observations after remediation.

| Finding | Retest status | Evidence |
|---|---|---|
| F-01 Wildcard CORS | Pending | To be captured |
| F-02 CSP | Pending | To be captured |
| F-03 Referrer Policy | Pending | To be captured |
| F-04 HSTS | Not applicable to local HTTP test | Production HTTPS verification required |

Status definitions:
- Fixed: The issue is no longer observed and the intended behavior is verified.
- Reduced: The risk has been lowered, but some concern remains.
- Unresolved: The original condition remains.
- Pending: No retest evidence is available.

## 8. Limitations

- Testing was limited to selected test cases.
- The application was hosted locally.
- No comprehensive penetration test or source-code audit was performed.
- Potential configuration observations were not treated as confirmed vulnerabilities without supporting evidence.
- Retest status must not be marked fixed without verification.

## 9. Conclusion

The documented assessment evaluated selected web security controls in OWASP Juice Shop. No exploitable vulnerability was confirmed by the available tests.

Several security configuration improvements were identified for review. Remediation and retest evidence will determine their final status.

## 7. Remediation and Retest Results

A local Nginx reverse proxy was configured on 127.0.0.1:3001.
The original Juice Shop container and baseline endpoint on port 3000
were left unchanged.

### 7.1 Changes applied

- Removed the upstream Access-Control-Allow-Origin wildcard header
  from the local proxy response.
- Added Referrer-Policy: strict-origin-when-cross-origin.
- Preserved X-Content-Type-Options: nosniff.
- Preserved X-Frame-Options: SAMEORIGIN.
- Removed duplicate security headers from the proxy response.

### 7.2 Retest results

| Test | Before | After | Result |
|---|---|---|---|
| HTTP response | 200 OK | 200 OK | Passed |
| Wildcard CORS header | Present | Absent | Changed at proxy |
| Referrer-Policy | Missing | Present | Added |
| X-Content-Type-Options | nosniff | nosniff | Preserved |
| X-Frame-Options | SAMEORIGIN | SAMEORIGIN | Preserved |
| Duplicate headers | Not applicable | None in captured response | Resolved |

Evidence:
- Baseline: evidence/baseline/headers-before.txt
- Retest: evidence/retest/headers-after.txt

### 7.3 Remaining observations

- Content-Security-Policy was not configured or tested.
- HSTS was not added because this lab endpoint uses local HTTP.
  HSTS should be evaluated on an HTTPS deployment.
- Removing wildcard CORS at the local proxy is a hardening change.
  It does not establish that an exploitable CORS vulnerability existed.
- The original Juice Shop container still has its original configuration.
  Changes were applied only at the local Nginx proxy.

## 8. Conclusion

The local remediation proxy returned HTTP 200 after configuration.
The wildcard CORS response header was removed at the proxy,
Referrer-Policy was added, and the existing security headers were
preserved without duplication.

No exploitable vulnerability was confirmed by the tests documented
in this assessment. Remaining observations are configuration
hardening recommendations and require further validation where applicable.
