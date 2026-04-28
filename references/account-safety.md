# Account Safety and Verification

Use this reference whenever Rufus capture requires logging into Amazon.

## Secret Handling

Never place account credentials, cookies, session tokens, OTP seeds, recovery codes, or one-time verification codes in:

- GitHub files,
- skill files,
- logs,
- screenshots intended for sharing,
- final reports,
- CSV or JSON capture outputs.

Prefer secrets supplied through environment variables, a local secrets file outside the repo, or the user's existing agent secret mechanism.

## Pre-Authorized Login Workflow

When a login or verification challenge appears:

1. Check whether the user already supplied an explicit login workflow for this account.
2. If username/password are available through the approved secret channel, use them.
3. If Amazon asks for an OTP/TOTP or email/SMS login code, check whether the user supplied an approved way to retrieve that code, such as a local TOTP secret, a pre-authorized OTP helper, or a user-provided verification service.
4. Use the code only for the current login attempt.
5. Record only non-sensitive metadata, such as `verification_action=preauthorized_otp_used`.
6. If the approved workflow fails, stop and ask for human help.

Do not expose or persist the OTP/TOTP secret. Do not include the verification provider credentials in the skill or repository.

## Challenge Types

Treat challenges differently:

- `login_required`: use the pre-authorized login workflow if available.
- `otp_or_totp_required`: use the pre-authorized OTP/TOTP workflow if available.
- `email_or_sms_code_required`: use the pre-authorized retrieval workflow if available.
- `captcha_or_robot_check`: do not attempt automated bypass. Use only a user-approved human-in-the-loop workflow, otherwise stop.
- `account_security_warning`: stop and report; do not continue collection.

## Logging Rules

Allowed:

- `logged_in=true`
- `verification_action=preauthorized_otp_used`
- `challenge_type=otp_or_totp_required`
- `capture_status=blocked`
- `failure_reason=no_preauthorized_verification_workflow`

Not allowed:

- account email or phone number,
- password,
- cookie or session values,
- OTP/TOTP seed,
- generated one-time code,
- screenshots that reveal sensitive account or verification details.

## Account Scope

Use credentials only for the user-authorized Amazon buyer account and only for front-end Rufus collection. Do not change account settings, profile persona details, addresses, payment settings, orders, reviews, or messages unless the user explicitly asks and confirms the exact action.
