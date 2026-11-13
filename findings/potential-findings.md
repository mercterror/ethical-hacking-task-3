# Potential Security Findings

## POT-01: Wildcard CORS Configuration

- **Affected component:** Juice Shop HTTP response
- **Observed value:** Access-Control-Allow-Origin: *
- **Status:** Requires further investigation
- **Evidence:** evidence/security-headers/response-headers.txt
- **Observation:** The application response includes a wildcard Access-Control-Allow-Origin header.
- **Potential impact:** Depending on the resources and data exposed, permissive cross-origin access may allow other websites to read application responses.
- **Root cause:** The server or application configuration permits requests from arbitrary origins.
- **Recommended action:** Restrict allowed origins to trusted domains where cross-origin access is required. Review sensitive endpoints and credential handling.
- **Conclusion:** This is not a confirmed vulnerability based on the response header alone.

## POT-02: Missing Security Headers

- **Affected component:** HTTP response
- **Observed values:** Content-Security-Policy, Strict-Transport-Security and Referrer-Policy were not present in the captured response.
- **Status:** Requires configuration review
- **Evidence:** evidence/security-headers/response-headers.txt
- **Observation:** These headers were not observed in the captured HTTP response.
- **Potential impact:** Depending on application behavior and deployment configuration, missing security headers may reduce browser-side security protections.
- **Recommended action:** Review and configure an appropriate Content Security Policy and Referrer Policy. Configure HSTS when the application is deployed over HTTPS.
- **Conclusion:** Missing headers alone do not establish an exploitable vulnerability.
