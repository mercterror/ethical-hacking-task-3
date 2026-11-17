---

## Task 4 – Penetration Test Report and Retest

**Objective:** Conduct an authorized penetration testing assessment of OWASP Juice Shop, document security observations, apply local proxy-level remediation, and verify the changes.

### Work completed
- Executive summary and technical methodology.
- Authentication, access-control and input-handling assessment.
- HTTP security-header analysis.
- Risk assessment and remediation recommendations.
- Local Nginx reverse proxy configuration.
- Before-and-after security-header retesting.
- Evidence of remediation and remaining limitations.

### Retest highlights
- Wildcard CORS response header removed at the local proxy.
- Referrer-Policy added and verified.
- Existing security headers preserved.
- HTTP 200 OK after remediation.

### Evidence and reports
- [Penetration Test Report](task-4/reports/penetration-test-report.md)
- [Remediation Plan](task-4/remediation/remediation-plan.md)
- [Baseline Evidence](task-4/evidence/baseline/headers-before.txt)
- [Retest Evidence](task-4/evidence/retest/headers-after.txt)

**Note:** Remediation applies to the local Nginx proxy, not the original Juice Shop container. No exploitable vulnerability was confirmed by the documented tests.# ethical-hacking-task-3
OWASP Web Testing Lab
