# Security Policy

Trading-system security failures can create direct financial loss. Credential handling is therefore part of EdgePilot's risk model, not merely an infrastructure concern.

## Secrets

Never commit or paste into tracked files:

- exchange API keys or secrets,
- wallet private keys or seed phrases,
- passwords,
- session tokens,
- webhook secrets,
- unrestricted cloud credentials,
- or personally sensitive account exports.

Use environment variables or an appropriate secret store when credentials are eventually required.

## Exchange API policy

When exchange integration is introduced:

- create dedicated credentials for EdgePilot rather than reusing unrelated credentials,
- grant only permissions required by the current phase,
- keep withdrawals disabled by default,
- prefer read-only credentials while building data/reconciliation capability,
- add trading permission only when a live deployment gate explicitly requires it,
- use IP restrictions/allowlists when supported and operationally appropriate,
- rotate credentials after suspected exposure,
- and never log secrets in application, test, CI, or experiment output.

## Funds custody

EdgePilot should not require wallet private keys merely to implement exchange trading. Any future feature that would require custody or withdrawal authority needs a separate security and risk review.

## Testing

Tests and examples should use synthetic/test credentials. No test should depend on a real secret being committed to the repository.

## Incident rule

If a credential may have been exposed, treat it as compromised: revoke/rotate it first, then investigate. Do not wait for proof of unauthorized use.
