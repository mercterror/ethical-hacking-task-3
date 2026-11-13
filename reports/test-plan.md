# OWASP Web Testing Lab - Test Plan

## Target
- Application: OWASP Juice Shop
- Environment: Local Docker container
- URL: http://127.0.0.1:3000
- Scope: Authorized local testing only

## Test Categories

### Authentication
- Login page inspection
- Successful login
- Registration workflow
- Invalid login test

### Access Control
- AC-01: Access profile route while logged out
- AC-02: Access basket route while logged out
- AC-03: Two-account basket isolation test (not yet confirmed)

### Input Handling
- INPUT-01: Normal search input (apple)
- INPUT-02: Unknown search input (test123xyz)
- INPUT-03: Special-character input (test & sample)

### Security Headers
- Captured HTTP response headers using curl.
- Reviewed CORS, content-type, framing and browser security headers.

## Limitations
- Testing was performed against a local training application.
- Results apply only to the tested environment and captured responses.
- No production systems were tested.
