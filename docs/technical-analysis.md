\# Technical Analysis



\## Vulnerability Class



Authorization bypass / permission mismatch.



\## Affected Area



GitLab GraphQL compliance management functionality.



\## Root Cause Pattern



A GraphQL mutation performed a write operation while being authorized with a permission intended for read-only compliance reporting.



The important security distinction is between:



\- Permission to view compliance violations

\- Permission to modify compliance violation state



A read permission should not authorize a mutation that persists a new violation status.



\## Security Boundary



The expected boundary was:



```text

Read-only compliance role

&#x20;       |

&#x20;       | allowed

&#x20;       v

View compliance reports and violations



Read-only compliance role

&#x20;       |

&#x20;       | should not be allowed

&#x20;       v

Modify violation status



The observed behavior crossed that boundary.



Impacted Security Property



Integrity.



The compliance violation status is audit-facing state. If a user with read-only access can modify it, compliance reports and governance workflows may no longer accurately represent the real state of violations.



Example Impact Scenario



An organization grants a user read-only access to compliance dashboards so that they can review violations.



That user should be able to inspect the violations, but not mark them as resolved or dismissed.



If the same user can update violation status through GraphQL, the read-only role effectively gains write capability over compliance state.



Recommended Fix Pattern



A mutation that changes compliance violation status should require a write/admin permission, or a dedicated update permission.



Possible approaches:



Require admin compliance permission



or:



Introduce a dedicated update\_compliance\_violation permission



Regression tests should verify that users with read-only compliance permissions cannot execute status-changing mutations.

