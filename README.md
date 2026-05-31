\# GitLab GraphQL Compliance Authorization Research



This repository documents a security research case study involving a GitLab GraphQL authorization boundary issue in the compliance management area.



The finding was independently identified and reported through HackerOne. The report was later closed as a duplicate because the same issue had already been submitted and was being tracked for resolution.



\## Summary



The research focused on a GraphQL mutation responsible for updating the status of project compliance violations.



The core observation was an authorization mismatch: a permission intended for read-only compliance visibility was also sufficient to execute a write operation that changed compliance violation state.



In practical terms, a user with a role intended to view compliance information could update the status of a compliance violation, for example from `DETECTED` to `RESOLVED` or `DISMISSED`.



\## Research Goal



The goal of this research was to verify whether read-only compliance permissions could cross a write boundary through GraphQL mutations.



The investigation focused on:



\- GraphQL authorization checks

\- Custom role permissions

\- Compliance dashboard abilities

\- Mutation behavior versus permission naming

\- Integrity impact on audit and compliance state



\## Impact



The issue affected integrity of compliance data.



A user expected to have read-only compliance access could alter compliance violation state. This could mislead audit workflows, governance reviews, or compliance dashboards that rely on violation status.



No confidentiality or availability impact was demonstrated.



\## Disclosure Status



The issue was responsibly reported via HackerOne.



The report was closed as duplicate because a prior report had already identified the same root cause and impact.



This repository does not include private HackerOne discussion, internal report identifiers beyond general duplicate status, live target data, secrets, or non-public attachments.



\## Repository Structure



```text

.

├── README.md

├── SECURITY.md

├── docs/

│   ├── research-timeline.md

│   ├── technical-analysis.md

│   └── responsible-disclosure.md

├── poc/

│   └── sanitized-rspec-poc.md

├── evidence/

└── screenshots/

Lessons Learned



This case highlights a recurring authorization pattern:



A permission named and designed for read access should not authorize a mutation that performs a persistent state change.



For GraphQL APIs, every mutation should be reviewed against the actual side effect it performs, not only against the object it accesses.



Responsible Research Notice



This repository is published for educational and professional portfolio purposes.



All details have been sanitized to avoid exposing private program information, secrets, customer data, or exploit material against live systems

