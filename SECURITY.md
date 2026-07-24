# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| main    | :white_check_mark: |

Only the `main` branch receives security updates.

## Reporting a Vulnerability

If you discover a security vulnerability in this project, please report it
privately so we can investigate and address it before public disclosure.

**Email:** security@joseguzman1337.dev (replace with your actual security contact)
**GitHub:** Open a [private security advisory](https://github.com/joseguzman1337/xss/security/advisories/new)
on this repository.

Please do **not** file a public GitHub issue for security-sensitive findings.

When reporting, please include:

- A clear description of the vulnerability and its impact.
- Steps to reproduce, or a proof-of-concept (PoC).
- The affected commit / version, if known.
- Any suggested mitigation, if available.

## Response Targets

| Stage            | Target         |
| ---------------- | -------------- |
| Initial triage   | within 7 days  |
| Status update    | within 14 days |
| Fix / mitigation | within 30 days |

We will acknowledge receipt of your report, assign a CVE if appropriate, and
coordinate disclosure timing with you.

## Safe Harbor

We will not pursue legal action against, request law enforcement investigation
of, or restrict your account based on activity performed in good-faith
research conducted under this policy. We consider research conducted under
this policy to be authorized access for the purposes of the Computer Fraud
and Abuse Act (CFAA) and equivalent laws.

Good-faith research means:

- You make a good-faith effort to avoid privacy violations and disruption
  of our services.
- You only interact with accounts you own or have explicit permission to
  access.
- You stop testing immediately if you encounter user data and report it to
  us so we can take steps to protect the affected users.
- You do not exploit a vulnerability for any reason beyond demonstrating it
  (no data exfiltration, no lateral movement, no persistence).

We reserve the right to revoke this safe harbor for activity that is
deceptive, destructive, or performed with intent to harm users.

## Out of Scope

The contents of `xss.md` and any branch named `*xss*` are intentionally
inert XSS payloads used for educational / CTF purposes. They are **not**
considered vulnerabilities; do not report them.

## Acknowledgements

We appreciate coordinated disclosure. Reporters who follow this policy will
be credited in the release notes (unless they prefer to remain anonymous).
