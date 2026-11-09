# Security Audit Case Study: Mobile Trading-Bot App (anonymised)

An authorised, static security review of a mobile trading-bot app. The app holds broker credentials and can place trades for users, so weaknesses in authentication and account protection matter more than in a typical app.

The project name, repository, file paths and commit details are left out on purpose. Findings that are not yet fixed are described only in general terms. The full report was shared privately with the owner.

## Stack reviewed

- API: NestJS, Prisma, PostgreSQL, Redis, BullMQ
- Mobile: Expo / React Native
- Worker: trading engine service
- Tooling: pnpm / Turborepo monorepo, Docker Compose, GitHub Actions CI

## Scope and method

- Static review of the source code, configuration and CI. No live testing and no requests sent to a running system.
- The project's own security spec was used as the checklist. For every rule it promised, I looked for the code that enforces it. Rules the code did not enforce became findings.
- Automated scans were used to find leads, then each lead was confirmed or rejected by reading the code.

## Tools

| Tool | Used for |
|------|----------|
| gitleaks | Secrets in the full git history |
| pnpm audit | Known vulnerable dependencies, traced by dependency path |
| Semgrep | OWASP Top 10, JWT and React rule sets |
| grep / sed / find | Manual review of auth, access control, crypto, logging and storage |
| GitHub CLI (gh) | Creating one tracked issue per finding for the owner |

## Results

17 findings: 0 critical, 0 high, 4 medium, 3 low-to-medium, 10 low, plus a short list of informational notes. No real secrets were found in the repository or its history.

The medium findings sit in step-up authentication, dependency updates and key management. Details are withheld until they are fixed.

## What was done well

- Argon2id password hashing
- Short-lived access tokens with refresh-token rotation
- Ownership checks on object lookups (no IDOR found)
- Broker credentials encrypted at rest
- Tokens kept in secure storage on the phone
- Log redaction for sensitive fields

## Lessons learned

1. **Check the code against the project's own spec.** Promises that are not enforced in code are the strongest findings.
2. **Severity is not priority.** A medium issue that protects real money can be first in the fix queue. Keep the two ratings separate.
3. **Scanner output is noise until traced.** A dependency alert in a build tool is not the same risk as one on the live server.
4. **Report responsibly.** Share findings privately, give the owner a tracked list, and publish details only after fixes.

## Next steps

Dynamic testing of a local instance, to demonstrate the top findings with real requests once the owner agrees.

## About

Written by Babalola Temilola. Authorised audit; all findings reported privately to the owner.
