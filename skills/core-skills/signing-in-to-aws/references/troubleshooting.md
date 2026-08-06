# AWS Login Troubleshooting

## CLI missing or too old

`aws login` requires AWS CLI 2.32.0 or later. Use the official [installation and update guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html). Ask before installing or updating software.

## Browser does not open

Use `aws login --remote` to obtain a URL and code for cross-device authentication.

## Permission error after login

The identity needs the `SignInLocalDevelopmentAccess` managed policy attached to its user, role, or group. Root users do not need it. Ask an administrator to grant it unless the user has authority to manage IAM themselves.

## Existing IAM Identity Center workflow

`aws sso login` and `aws login` are different workflows. Profiles with an SSO start URL should continue using `aws sso login --profile <name>`. For failures, check session expiry, revoked authorization, cached tokens under `~/.aws/sso/cache/`, and Identity Center configuration.

## Existing long-term access keys

If `aws configure list` shows an `AKIA` access key, explain that it is a long-lived secret stored on disk. Prefer short-term credentials unless the user explicitly chooses otherwise. Never print or expose the secret key.

## GovCloud and China

`aws login` is not available in AWS GovCloud or AWS China regions. Mention this only when the user's partition makes it relevant.

## Fallback to `aws configure`

Do not present `aws configure` as a peer default. Offer it only when the user declines `aws login`, asks for alternatives, or uses an unsupported partition. Explain that long-term keys persist on disk and do not expire automatically.

## References

- [Sign in through the AWS CLI](https://docs.aws.amazon.com/signin/latest/userguide/command-line-sign-in.html)
- [AWS CLI installation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [SignInLocalDevelopmentAccess policy](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/SignInLocalDevelopmentAccess.html)
- [IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
