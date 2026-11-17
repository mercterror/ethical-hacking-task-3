# Remediation Plan and Retest Record

## F-01: Wildcard CORS

- [x] Remove wildcard CORS response header at local Nginx proxy.
- [x] Verify that the wildcard header is absent from the proxy response.
- [ ] Review application cross-origin requirements and sensitive endpoints.
- Status: Local proxy change verified; original application configuration unchanged.

## F-02: Content Security Policy

- [ ] Design a policy compatible with the application.
- [ ] Test in report-only mode where supported.
- [ ] Check browser console errors and application functionality.
- [ ] Capture the response header after implementation.
- Status: Pending.

## F-03: Referrer Policy

- [x] Configure strict-origin-when-cross-origin at local Nginx proxy.
- [x] Verify the response header.
- Status: Verified at local proxy.

## F-04: HSTS

- [ ] Confirm HTTPS is configured in the production environment.
- [ ] Configure HSTS only for the HTTPS deployment.
- [ ] Verify the response over HTTPS.
- Status: Not applicable to the current local HTTP test.

## Retest Record — 17 November 2026

| Item | Retest result |
|---|---|
| HTTP response | 200 OK |
| Wildcard CORS | Absent at local proxy |
| Referrer-Policy | Added and verified |
| X-Content-Type-Options | nosniff preserved |
| X-Frame-Options | SAMEORIGIN preserved |
| Duplicate headers | None observed |
| CSP | Pending |
| HSTS | Not applicable to local HTTP test |

Evidence:
- Baseline: evidence/baseline/headers-before.txt
- Retest: evidence/retest/headers-after.txt

## Scope Limitation

These changes apply only to the local Nginx proxy at 127.0.0.1:3001. They do not modify the original Juice Shop container at 127.0.0.1:3000 or establish that the original application had an exploitable vulnerability.

Do not mark an item fixed unless the relevant behavior has been retested and evidence saved.
