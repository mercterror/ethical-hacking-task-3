# OWASP Web Testing Lab - Final Report

## 1. Introduction

This project evaluates selected web application security controls using OWASP Juice Shop, an intentionally vulnerable training application.

All testing was performed against a locally hosted Docker instance in an isolated lab environment.

## 2. Scope

Target: OWASP Juice Shop
URL: http://127.0.0.1:3000

Testing categories:
- Authentication
- Access control
- Input handling
- HTTP security headers

## 3. Methodology

Manual browser testing was used for authentication, access control and input handling. Curl was used to capture HTTP response headers.

## 4. Test Results

### Authentication

The login page was inspected. Successful login and registration were observed. An invalid-login test was performed, but its exact response was not recorded.

No authentication vulnerability was confirmed from the available evidence.

### Access Control

- AC-01: Logged-out profile route did not display protected profile information.
- AC-02: Logged-out basket route displayed an empty basket.
- AC-03: Two accounts displayed separate baskets.

No unauthorized data access was observed in these tests.

### Input Handling

| Test | Input | Observed result |
|---|---|---|
| INPUT-01 | apple | Normal search results appeared |
| INPUT-02 | test123xyz | No results appeared |
| INPUT-03 | test & sample | No results appeared |

No injection vulnerability was confirmed by these basic search tests.

### Security Headers

The captured HTTP response returned status 200 OK.

Observed headers:
- X-Content-Type-Options: nosniff
- X-Frame-Options: SAMEORIGIN
- Access-Control-Allow-Origin: *
- Feature-Policy: payment 'self'

The following headers were not observed in the captured response:
- Content-Security-Policy
- Strict-Transport-Security
- Referrer-Policy

The wildcard CORS configuration and missing headers require further review. They are recorded as potential security concerns, not confirmed vulnerabilities.

## 5. Evidence

Evidence is stored in the evidence directory:

- evidence/authentication/
- evidence/access-control/
- evidence/input-handling/
- evidence/security-headers/response-headers.txt

## 6. Recommendations

1. Restrict CORS to trusted origins when cross-origin access is required.
2. Review sensitive endpoints for unintended cross-origin data exposure.
3. Configure an appropriate Content Security Policy.
4. Configure a suitable Referrer Policy.
5. Enable HSTS when deployed over HTTPS.
6. Continue testing authentication, authorization and input validation with additional test cases.

## 7. Limitations

- Testing was restricted to the local training environment.
- Only the documented test cases were performed.
- No production system was tested.
- The absence of a confirmed vulnerability in these tests does not prove that the application is secure.

## 8. Conclusion

The project successfully tested selected authentication, access-control, input-handling and HTTP-header behaviors in OWASP Juice Shop.

The recorded tests did not confirm an exploitable vulnerability. Potential configuration concerns were documented for further investigation.
