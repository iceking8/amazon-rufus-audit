# Account Safety and Verification

Use this reference whenever Rufus capture requires logging into Amazon.

## Secret Handling

Never place account credentials, cookies, session tokens, OTP seeds, recovery codes, phone numbers, or one-time verification codes in:

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
3. If Amazon asks for an OTP/TOTP, email/SMS login code, or mobile phone verification code, check whether the user supplied an approved way to retrieve that code, such as a local TOTP secret, a pre-authorized OTP helper, a user-provided verification service, or a human-provided SMS code.
4. Use the code only for the current login attempt.
5. Record only non-sensitive metadata, such as `verification_action=preauthorized_otp_used`.
6. If the approved workflow fails, stop and ask for human help through the agent's normal user-facing reply.

Do not expose or persist the OTP/TOTP secret. Do not include the verification provider credentials in the skill or repository.
Do not use terminal stdin, Python `input()`, or other blocking prompts for credentials, OTP codes, overwrite confirmations, or human intervention.

If the browser is not logged into an Amazon buyer account and no approved login workflow is available, stop all capture work and ask the human for the Amazon buyer account login method through the normal user-facing or secret channel. Do not keep probing product pages or Rufus while logged out.

## Mobile Number Verification

If Amazon forces an "Add a mobile number" or mobile verification page:

1. Treat it as an account security challenge, not a Rufus visibility issue.
2. Stop all Rufus and Listing capture work.
3. Ask the human for the buyer account phone number through the normal user-facing or approved secret channel.
4. Enter the phone number only for this Amazon verification step.
5. After Amazon accepts the phone number, tell the human exactly `已添加手机号`.
6. Ask the human for the SMS verification code Amazon sent to that phone.
7. Enter the SMS code once, verify the challenge clears, and resume only after `login_status=logged_in`.

Do not store or log the phone number or SMS code. If the phone number or SMS code fails, stop and ask for human help instead of looping.

## Challenge Types

Treat challenges differently:

- `login_required`: use the pre-authorized login workflow if available.
- `otp_or_totp_required`: use the pre-authorized OTP/TOTP workflow if available.
- `email_or_sms_code_required`: use the pre-authorized retrieval workflow if available.
- `mobile_number_required`: stop capture, ask human for the phone number, then ask for the SMS code after submission.
- `mobile_number_code_required`: ask human for the received SMS code and use it only for the current verification attempt.
- `captcha_or_robot_check`: do not attempt automated bypass. Use only a user-approved human-in-the-loop workflow, otherwise stop.
- `account_security_warning`: stop and report; do not continue collection.

## Logging Rules

Allowed:

- `logged_in=true`
- `login_status=not_logged_in`
- `verification_action=preauthorized_otp_used`
- `verification_action=human_phone_submitted`
- `verification_action=human_sms_code_submitted`
- `challenge_type=otp_or_totp_required`
- `challenge_type=mobile_number_required`
- `challenge_type=mobile_number_code_required`
- `failure_reason=amazon_buyer_login_required`
- `failure_reason=mobile_number_verification_required`
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
