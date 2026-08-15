# Security Policy

Security is part of DevHarness's architecture, not a final-stage add-on. The project assumes that models, tools, dependencies, commands, and external systems may fail or behave unexpectedly.

## Supported versions

DevHarness has not released an implementation. No version is currently supported for production use.

When releases begin, this section will list the supported release lines and security update policy.

## Public repository publication rules

DevHarness is a public repository. Published code, prompts, logs, traces, fixtures, examples, documentation, commit history, issues, and pull requests must not contain:

- credentials, tokens, keys, cookies, or other authentication material;
- personal, customer, or confidential data;
- database, memory, filesystem, or diagnostic dumps;
- private endpoints, internal hostnames, infrastructure details, or non-public repository locations;
- any other artifact whose disclosure could create security, privacy, legal, or operational risk.

Use synthetic data or the minimum thoroughly sanitized excerpt needed to demonstrate a behavior. Redaction must remove the sensitive value rather than merely obscure its label. Before publication, inspect the staged diff and relevant generated output. If sensitive material is exposed, stop further sharing, privately notify the maintainer through the reporting path below, and rotate or revoke affected credentials; deleting the latest file alone does not remove it from Git history.

## Reporting a vulnerability

Do not open a public issue for a suspected vulnerability, exposed credential, unsafe command path, sandbox escape, authorization bypass, or data-loss condition.

Use GitHub's private vulnerability reporting feature for this repository when it is enabled. If that feature is unavailable, contact the repository owner privately through the contact method published on the owner's GitHub profile. Do not include live secrets or unnecessary personal data in the first message.

Please include, when safely possible:

- affected component and revision;
- impact and required preconditions;
- minimal reproduction steps using non-production data;
- evidence such as sanitized logs or traces;
- whether the issue is known to be actively exploited;
- suggested mitigation, if available.

You should receive an acknowledgment target within seven days after a functioning private reporting channel is available. A remediation timeline will depend on severity, exploitability, and release status. Coordinated disclosure is preferred.

## Security invariants

The implementation must preserve these invariants:

- least privilege by default;
- explicit separation of control, execution, and provider authority;
- deny-by-default treatment of destructive or out-of-scope commands;
- no direct experimentation on the only valid project state;
- no production resource as an unprotected test target;
- secret redaction in logs, traces, checkpoints, memory, and model context;
- independent verification before state promotion;
- auditable decisions and command execution;
- safe rollback without silent loss of valuable work;
- provider adapters cannot override project policy.

Failures of these invariants are security issues even when no traditional remote exploit is involved.

## Out of scope for public testing

Without explicit written authorization, do not test against accounts, repositories, services, or infrastructure you do not own; do not perform denial-of-service testing; and do not access, retain, or publish other people's data.
