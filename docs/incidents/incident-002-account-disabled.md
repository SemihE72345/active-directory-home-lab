# Incident 002 — User account disabled

**Type:** Simulated account lifecycle / help-desk audit exercise  
**Affected identity:** `CORP\nanderson` (Noah Anderson)  
**Domain controller:** `DC01.corp.adlab.test`  
**Evidence:** [Screenshot 10 — Event ID 4725](../../screenshots/10-account-management-audit.png)

## Scenario

A fictional Sales employee account was intentionally disabled during a controlled administrative exercise. The goal was to establish which account was changed and which administrator performed the action.

## Evidence reviewed

- Windows Security Event ID: **4725** — a user account was disabled.
- Target username: **`nanderson`**; target domain: **`CORP`**.
- Subject username: **`Administrator`**; subject domain: **`CORP`**.
- Computer recording the event: **`DC01.corp.adlab.test`**.
- Event time shown in UTC: **2026-11-04 01:44:26Z**.

## Investigation and response

1. Disabled Noah Anderson's test account through Active Directory Users and Computers.
2. Confirmed the account's disabled state with Active Directory PowerShell.
3. On DC01, located Event ID 4725 in **Event Viewer → Windows Logs → Security**.
4. Reviewed target-account and subject-account fields to associate the action with the authorised domain Administrator.
5. Re-enabled the account after the exercise; the user confirmed restoration of the test accounts.

## Assessment

The logged account disablement aligns with a **known administrator-initiated lab operation**, rather than an unexplained malicious change. In a real organisation, this event should be matched to a help-desk ticket or an authorised offboarding request.

## Skills demonstrated

Account lifecycle management, audit trail review, administrator attribution through event fields, and recovery checks.
