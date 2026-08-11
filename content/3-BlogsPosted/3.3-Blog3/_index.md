---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Research Notes on Reducing SMS OTP Fraud with Vonage and Amazon Cognito

## Context and problem

User authentication is one of the most attacked touchpoints in applications. According to figures in the article, global cybercrime costs may reach **$23 trillion by 2027**; **20%** of fraud relates to synthetic identity and authentication exploits; account takeover (ATO) rose **141%** since 2021.

But the problem is not only security. **SMS OTP** (one-time passcodes via text) achieves only about **80% conversion** — meaning **1 in 5 legitimate users abandons** at verification. Enterprises also absorb hundreds of thousands of helpdesk tickets annually because users never receive or mistype codes.

The main message I took away: the industry long assumed **stronger security means worse UX**. Vonage argues that is a limitation of older tools, not a fixed law. **Real-time mobile network operator (MNO) data** can improve identity assurance and reduce friction at the same time.

## How is network-powered different from traditional tools?

Most fraud signals today come from **aggregated, cached, or behavioral data** — phone lookups from stale databases, spoofable device fingerprints, behavioral models needing session history.

Vonage **network-powered** queries **mobile operators directly in real time**:

- Check **SIM swap** → ask the network that performed the swap.
- **Silent Authentication** → prove SIM possession via the **cellular data session**, not SMS.

For ATO via SIM swap, "recently" means **minutes to hours** — weekly-refreshed static databases log events after the fact, not in time to block. Real-time operator queries close that window.

## Vonage's three pillars

Vonage combines three APIs into a security stack integrated with Cognito through the **CUSTOM_AUTH** flow:

### 1. Identity Insights — pre-verification checks

Runs **before** verification channels start. Signals I noted:

| Signal | Meaning in my understanding |
|--------|----------------------------|
| format, network_type | Filter invalid, VoIP, landline, premium-rate numbers — common in synthetic accounts and bot fraud |
| sim_swap | Detect SIM swaps within a configurable window — leading ATO indicator |
| subscriber_match | Match name/address against operator KYC records |
| device_swap *(coming soon)* | Recent device change on a number may mean SIM compromise |
| recycled_number *(coming soon)* | Reassigned numbers risk false identity matches at onboarding |

Pre-checks trigger **risk policy**: step-up challenge, hard block, or silent logging. Fraud is blocked **before a single OTP is sent** — saving SMS cost and fraud processing overhead.

### 2. Verify + Silent Authentication — reducing the friction tax

The article calls cumulative loss from each extra auth step the **friction tax**. With ~80% SMS OTP completion, roughly 20% of legitimate users leave before entering the app.

**Silent Authentication:** when a user signs in from a mobile device, Vonage routes an HTTP request over the user's **cellular data connection**. The operator confirms the SIM registered to the phone number matches the session — happens **in the background, under 5 seconds**, with **no code entry**.

If Silent Auth is unavailable, Verify **automatically falls back** to SMS, RCS, Voice, WhatsApp, or email — transparent to the user.

Clear benefit: reduces the three main SMS OTP attack vectors — **SIM swap**, **SS7 interception**, and **social engineering** — without extra user input.

### 3. Fraud Defender — protecting the OTP channel

Counters **AIT (Artificially Inflated Traffic)** and **SMS pumping** — bots sending mass OTPs to premium-rate numbers attackers control, generating large costs that can run undetected.

Fraud Defender monitors and blocks in real time at send time. Vonage reports customers saved **over $3M** in SMS fraud costs; for Verify API customers, Fraud Defender is **included at no extra cost**.

## Integration architecture with Amazon Cognito

![Risk-adaptive sign-in architecture: CloudFront/WAF → API Gateway → Cognito → Lambda → Vonage → MNO](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2026/06/09/ARCHBLOG-1533-1.png)

*Figure 1 — Risk-adaptive authentication with Cognito and Vonage*

Five layers I summarized:

| Layer | Role |
|-------|------|
| Client app (mobile/web) | Starts CUSTOM_AUTH, opens check_url over cellular network, submits verification code to Cognito |
| Amazon Cognito User Pool | Orchestrates challenge flow, issues JWT on success |
| 3 Lambda triggers | Define Auth Challenge, Create Auth Challenge (calls Vonage), Verify Auth Challenge |
| Vonage APIs | Identity Insights + Verify + Fraud Defender |
| Mobile Network Operators | SIM-level verification via CAMARA/Open Gateway |

![Sequence diagram: login flow with SIM-swap pre-check and Silent Auth](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2026/06/09/ARCHBLOG-1533-2.png)

*Figure 2 — Login flow via CUSTOM_AUTH*

Six steps I recorded:

1. Client calls InitiateAuth with CUSTOM_AUTH, passing the phone number.
2. **Define Auth Challenge** Lambda → Cognito issues CUSTOM_CHALLENGE.
3. **Create Auth Challenge** Lambda → calls Identity Insights → if pass, calls Verify Silent Auth → returns check_url to client.
4. Client opens HTTPS to check_url → redirects through carrier network → receives verification code.
5. Client calls RespondToAuthChallenge with the code.
6. **Verify Auth Challenge** Lambda → confirms with Vonage → Cognito issues session token.

What I valued: **no need to change existing user pool or app client** — plug into CUSTOM_AUTH, deploy with sam deploy. Phased rollout is possible: start with high-risk journeys (password recovery, high-value transactions), expand to daily login.

## Risk-aware authentication by journey

| Journey | Risk | Vonage workflow (summary) |
|---------|------|---------------------------|
| New account signup | CRITICAL | Filter non-mobile numbers + Subscriber Match KYC → Silent Auth zero-tap |
| Daily login | MEDIUM | SIM swap + device consistency → passive Silent Auth, step-up on elevated signals |
| Password recovery, profile/2FA changes | HIGH | Mandatory SIM swap hard-check + Subscriber Match → Silent Auth required |
| High-value transaction | CRITICAL | Full signal stack → Silent Auth + secondary challenge if risk elevated |

Low-risk actions (view account, browse content) add **no friction**. Sensitive actions trigger the full assurance stack — **policy-driven**, configurable per journey.

## Implementation and compliance notes

**Prerequisites:** AWS account (Cognito, Lambda, Secrets Manager, CloudWatch, WAF), Cognito user pool, Vonage API account, AWS SAM CLI, Silent Auth SDK on mobile.

**Security (Well-Architected Security Pillar):**
- IAM least privilege per Lambda.
- TLS 1.2+; CloudWatch logs + CloudTrail for Cognito API audit.
- AWS WAF rate limiting on auth endpoints against brute-force.
- Vonage credentials in **AWS Secrets Manager**.

**Privacy:** PII does not leave the operator — Subscriber Match returns only a **match score**; Silent Auth exchanges no PII; the **cellular session is the credential**. Supports GDPR, PSD2/SCA, HIPAA, DORA, CCPA per the original post.

**Production results — Lydia Solutions (European fintech):** deployed October 2024; up to **50%** latency reduction vs previous auth; **2–8.5%** conversion improvement vs SMS-only in other deployments; **50–75%** auth journey latency reduction.

## Connection to my internship project

Law-Chatbot uses Cognito for web app auth — I mainly configured the user pool, app client, and JWT validation on FastAPI. The Vonage article broadened my view: auth is not just "login succeeded" but **context-aware risk decisions**.

If the legal chatbot later offers **personal case lookup** or **account-bound advice**, I would consider:
- SIM swap pre-check before sensitive sessions;
- Silent Auth for a mobile app instead of plain SMS OTP;
- WAF + rate limiting in front of auth APIs — patterns I did not go deep on during the internship.

The article also reminded me of **hidden OTP costs**: not only fraud but user drop-off and helpdesk load — metrics worth measuring when evaluating auth solutions.

## Conclusion

Instead of applying the same verification friction to every session, enterprises can use **real-time network signals** to decide: verify silently when safe, step up when risk appears, block when fraud is detected.

The Vonage stack (Identity Insights + Verify + Fraud Defender) plus Cognito CUSTOM_AUTH shows **security and UX are not mutually exclusive** — deployable with three Lambdas, starting from high-risk journeys and expanding. Useful knowledge for designing Cognito-based, mobile-first systems after graduation.

### Reference

- [Reducing SMS OTP fraud with Vonage network-powered solutions and Amazon Cognito](https://aws.amazon.com/blogs/architecture/reducing-sms-otp-fraud-with-vonage-network-powered-solutions-and-amazon-cognito/)
- [Facebook post (AWS Study Group FCAJ)](https://www.facebook.com/groups/awsstudygroupfcj/posts/2240609806703979/)
