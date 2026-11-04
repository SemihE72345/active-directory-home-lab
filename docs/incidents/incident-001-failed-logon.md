# Incident 001 — Failed interactive domain logon

**Type:** Simulated IT help-desk / security triage exercise  
**Affected identity:** `CORP\jwilson`  
**Endpoint:** `CLIENT01.corp.adlab.test`  
**Evidence:** [Screenshot 09 — Event ID 4625](../../screenshots/09-failed-login-investigation.png)

## Scenario

A fictional employee, James Wilson, failed to sign in to the Windows 11 workstation during a controlled password-error exercise. The goal was to locate the audit record and interpret the failure without assuming malicious intent.

## Evidence reviewed

- Windows Security Event ID: **4625** — an account failed to log on.
- Account for which logon failed: **`jwilson`**, domain **`CORP`**.
- Computer: **`CLIENT01.corp.adlab.test`**.
- Logon type: **2** — interactive.
- Failure status: **`0xC000006D`**; substatus **`0xC000006A`**, associated with an incorrect password.
- The event describes a local interactive sign-in failure; the evidence does not identify an attacker.

## Investigation and response

1. Generated a small number of failed sign-in attempts on the isolated workstation using the fictional test identity.
2. Used **Event Viewer → Windows Logs → Security** on CLIENT01 and filtered for Event ID 4625.
3. Matched the event's username, machine, logon type and failure code to the expected exercise.
4. Verified valid domain-user authentication separately on CLIENT01 (see [Screenshot 07](../../screenshots/07-domain-user-authentication.png)).

## Assessment

Consistent with an **incorrect password during an interactive sign-in attempt**. This controlled event alone is insufficient to establish password spraying or compromise. In a real incident, triage would consider recurrence, additional source devices, timing and corroborating domain-controller events.

## Skills demonstrated

Security-event triage, Windows authentication failure-code interpretation, evidence correlation, and service restoration verification.
