# Penetration Test Report and Retest

## 1. Executive Summary

This assessment evaluates selected security controls in an OWASP Juice Shop application running in an isolated local Docker environment.

The assessment covered authentication, access control, input handling and HTTP security headers.

No exploitable vulnerability was confirmed by the tests performed. Several security configuration observations were identified, including a wildcard CORS response header and missing recommended browser security headers.

A local Nginx reverse proxy was configured to remove the wildcard CORS response header and add a Referrer-Policy header. Retesting confirmed the changes while preserving existing security headers.

## 2. Scope and Authorization

- Application: OWASP Juice Shop
- Environment: Local Docker lab
- Original URL: http://127.0.0.1:3000
- Remediation proxy: http://127.0.0.1:3001
- Testing type: Authorized educational security assessment
- Scope: Authentication, access control, input handling and HTTP response headers

No external or production systems were tested.

## 3. Technical Methodology

1. Review the application and identify the testing scope.
2. Inspect login and registration behavior.
3. Test profile and basket routes while logged out.
4. Compare basket contents between two test accounts.
5. Test normal, unknown and special-character search inputs.
6. Capture HTTP response headers using cURL.
7. Review observations and assign preliminary risk classifications.
8. Configure a local Nginx remediation proxy.
9. Capture and compare response headers before and after remediation.

## 4. Findings and Risk Assessment

No exploitable vulnerability was confirmed during the documented tests.

The following items are configuration observations and hardening recommendations, not confirmed vulnerabilities.

### F-01: Wildcard CORS Response Header

- Category: Security configuration
- Preliminary severity: Low – requires validation
- Evidence: Task 3 evidence/security-headers/response-headers.txt
- Observation: Access-Control-Allow-Origin: * was present.
- Potential impact: Depending on exposed resources and credential configuration, permissive cross-origin access may allow other origins to read application responses.
- Recommendation: Restrict cross-origin access to explicitly trusted origins where required.
- Status: Hardening change verified at local Nginx proxy. Exploitability not established.

### F-02: Content Security Policy Not Observed

- Category: Browser security configuration
- Preliminary severity: Informational
- Observation: Content-Security-Policy was not present in the captured baseline response.
- Recommendation: Design and test an application-compatible Content Security Policy.
- Status: Pending; not implemented.

### F-03: Referrer Policy Not Observed

- Category: Browser security configuration
- Preliminary severity: Informational
- Observation: Referrer-Policy was not present in the captured baseline response.
- Recommendation: Configure an appropriate policy, such as strict-origin-when-cross-origin.
- Status: Added and verified at local Nginx proxy.

### F-04: HSTS Not Observed

- Category: Transport security configuration
- Preliminary severity: Informational for local HTTP testing
- Observation: Strict-Transport-Security was not present.
- Recommendation: Enable HSTS on an HTTPS production deployment after confirming HTTPS works correctly.
- Status: Not applicable to the current local HTTP test.

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

## 6. Remediation Applied

A local Nginx reverse proxy was configured at 127.0.0.1:3001. The original Juice Shop container and port 3000 baseline endpoint were left unchanged.

Changes:
- Removed the upstream Access-Control-Allow-Origin wildcard header from the local proxy response.
- Added Referrer-Policy: strict-origin-when-cross-origin.
- Preserved X-Content-Type-Options: nosniff.
- Preserved X-Frame-Options: SAMEORIGIN.
- Removed duplicate security headers from the proxy response.

## 7. Retest Results

| Test | Before | After | Result |
|---|---|---|---|
| HTTP response | 200 OK | 200 OK | Passed |
| Wildcard CORS header | Present | Absent | Changed at proxy |
| Referrer-Policy | Missing | Present | Added at proxy |
| X-Content-Type-Options | nosniff | nosniff | Preserved |
| X-Frame-Options | SAMEORIGIN | SAMEORIGIN | Preserved |
| Duplicate headers | Not applicable | None observed | Resolved |

Evidence:
- Baseline: evidence/baseline/headers-before.txt
- Retest: evidence/retest/headers-after.txt

### Remaining Retest Items

- Content-Security-Policy: Not implemented; compatibility testing remains pending.
- HSTS: Not tested because the local endpoint uses HTTP. Verify on an HTTPS deployment.

## 8. Limitations

- Testing was limited to selected test cases.
- The application was hosted locally.
- No comprehensive penetration test or source-code audit was performed.
- Potential configuration observations were not treated as confirmed vulnerabilities without supporting evidence.
- Remediation was applied only to the local Nginx proxy, not to the original Juice Shop container.
- Removing wildcard CORS at the proxy does not establish that an exploitable CORS vulnerability existed.

## 9. Conclusion

The assessment evaluated selected web security controls in OWASP Juice Shop. No exploitable vulnerability was confirmed by the documented tests.

The local Nginx proxy returned HTTP 200 after remediation. The wildcard CORS response header was removed, Referrer-Policy was added, and existing security headers were preserved without duplication.

Further CSP compatibility testing and HTTPS production hardening remain recommended.
