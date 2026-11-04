# Incident 003 — Domain account lockout

**Type:** Simulated authentication protection / security triage exercise  
**Target identity:** `lockouttest` (dedicated fictional test account)  
**Domain controller:** `DC01.corp.adlab.test`  
**Evidence:** [Screenshot 11 — Event ID 4740](../../screenshots/11-account-lockout-investigation.png)

## Scenario

A lab account-lockout policy was configured and intentionally triggered by incorrect test logons. The objective was to locate the associated directory security event, recognise the affected identity and restore the test environment.

## Evidence reviewed

- Windows Security Event ID: **4740** — a user account was locked out.
- Target username: **`lockouttest`**.
- Computer recording the event: **`DC01.corp.adlab.test`**.
- Event data includes **`CLIENT01`** in `TargetDomainName`, while `SubjectUserName` is **`DC01$`**. Because the supplied view does not show an explicit caller-computer field, it should **not** be presented as independent proof of the original logon source.
- Event time shown in UTC: **2026-11-04 01:56:01Z**.

## Investigation and response

1. Configured an Active Directory account-lockout policy for the isolated lab and used a dedicated `lockouttest` identity.
2. Generated controlled unsuccessful sign-in attempts, stopping when the account locked.
3. Inspected DC01's Security log for Event ID 4740 and reviewed the target-account fields.
4. Verified the locked state with `Get-ADUser -Identity lockouttest -Properties LockedOut`.
5. Unlocked the test identity with `Unlock-ADAccount` and later restored/disabled test accounts as appropriate.

## Assessment

The audit entry documents a **lab-induced account lockout**. A real investigation would correlate domain-controller events, Kerberos failures, endpoint events and caller-computer fields before concluding where the failures originated.

## Skills demonstrated

Defensive account policy testing, interpreting Event ID 4740, recognising evidence limitations, and controlled account recovery.
