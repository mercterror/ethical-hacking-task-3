# Confirmed Security Findings

No exploitable security vulnerabilities were confirmed during the tests documented in this report.

The following tests were completed and recorded:

## Authentication
- Login page inspected.
- Successful login observed.
- Registration success observed.
- Invalid login test performed; exact response not recorded.

## Access Control

### AC-01: Logged-out profile access
- Result: Profile route opened, but protected profile information was not displayed.
- Conclusion: No unauthorized profile-data exposure observed.

### AC-02: Logged-out basket access
- Result: Basket page displayed an empty basket.
- Conclusion: No unauthorized basket-data exposure observed.

### AC-03: Basket isolation between two accounts
- Result: The two accounts displayed separate baskets.
- Conclusion: No cross-account basket exposure observed in this test.

## Input Handling

### INPUT-01: Normal search
- Input: apple
- Result: Normal search results appeared.
- Conclusion: Normal search functionality worked.

### INPUT-02: Unknown search term
- Input: test123xyz
- Result: No results appeared.
- Conclusion: No unexpected behavior was reported.

### INPUT-03: Special characters
- Input: test & sample
- Result: No results appeared.
- Conclusion: No unexpected behavior was reported. This test does not establish whether the application is vulnerable to injection.

## Overall Conclusion

No exploitable vulnerability was confirmed by the tests recorded above. This conclusion is limited to the specific test cases performed and does not establish that the application is free from vulnerabilities.
