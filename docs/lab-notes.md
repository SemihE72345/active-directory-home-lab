# Lab build notes

## Configuration

- **Server:** `DC01`, Windows Server 2025 Evaluation, domain `corp.adlab.test`, IP `192.168.10.10/24`, preferred DNS `192.168.10.10`.
- **Client:** `CLIENT01`, Windows 11 Enterprise Evaluation, IP `192.168.10.20/24`, preferred DNS `192.168.10.10`.
- **Networking:** Both attached to a VirtualBox **Internal Network** called `AD-LAB` with no default gateway. Installed Windows using evaluation ISOs, not production licenses.
- **Directory:** Created `AdLab-Technologies` OU containing `IT`, `HR`, `Finance`, `Sales`, `Groups`; later added `Workstations` for GPO targeting.
- **Users:** 8 fictional employee identities (2 per department); each in the relevant departmental OU.
- **Groups:** Global security groups `GG_IT_Users`, `GG_HR_Users`, `GG_Finance_Users`, `GG_Sales_Users`. Memberships assigned by department; no privileged domain group membership granted to standard employees.
- **Policy:** `GPO-Workstation-Security` linked to `Workstations` OU; `Interactive logon: Machine inactivity limit` set to 300 seconds. A separate domain account-lockout policy was configured/tested for the exercise.

## Verification commands

Run in appropriate elevated shells, using the legitimate lab administrator account where needed.

```powershell
# DC01
Get-ADUser -Filter * -SearchBase 'OU=AdLab-Technologies,DC=corp,DC=adlab,DC=test' | Select Name,SamAccountName,Enabled
Get-ADGroup -Filter 'Name -like "GG_*"' | Select Name,GroupScope,GroupCategory
Get-ADComputer -Identity CLIENT01 | Select Name,Enabled,DNSHostName
Get-ADDefaultDomainPasswordPolicy | Select LockoutThreshold,LockoutDuration,LockoutObservationWindow
```

```cmd
:: DC01
ipconfig /all
dcdiag /test:dns /DnsBasic /v
nltest /dsgetdc:corp.adlab.test

:: CLIENT01
whoami
hostname
gpresult /r /scope computer
```

## Good operational practices

- Keep both lab VMs isolated from physical networks where possible.
- Take account of limited host storage (the virtual disks grow over time if dynamically allocated).
- Protect administrator credentials, do not commit passwords, and never upload complete VM disk images or sensitive real records to a public repository.
- Capture only relevant event data; avoid treating simulated logon failures as proven malicious behaviour.
- Note the Windows evaluation expiry and the earlier `nslookup` timeout behaviour if discussing limitations in interviews.
