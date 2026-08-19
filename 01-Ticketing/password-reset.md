# 🎫 Ticket #001 — Password Reset & Local Account Troubleshooting

> **Scenario:** A local Windows user cannot access their account because they do not know their password.

---

## 🧩 Environment

* Windows workstation
* PowerShell
* Local Windows accounts
* Administrative account access

---

## 👤 Test User

**Name:** Jordan Smith
**Username:** `jsmith`

The account was created specifically for this Help Desk lab.

---

## 🔎 Investigation

I started by reviewing the existing local Windows accounts:

```powershell
Get-LocalUser
```

I then created a new local test account for Jordan Smith.

During creation, I made a syntax mistake that resulted in the account being created as:

```text
-NameJsmith
```

instead of:

```text
jsmith
```

Rather than deleting the account and starting over, I identified the incorrect username and corrected it with:

```powershell
Rename-LocalUser -Name "-NameJsmith" -NewName "jsmith"
```

I then verified the account:

```powershell
Get-LocalUser -Name "jsmith"
```

✅ The user now existed under the correct username.

---

## 🔐 Checking Account Security

Next, I inspected the account configuration:

```powershell
Get-LocalUser -Name "jsmith" |
Select-Object Name,Enabled,PasswordRequired,PasswordExpires,LastLogon
```

The account showed:

```text
Enabled          : True
PasswordRequired : False
```

Since this lab was meant to simulate a normal employee account, I changed the configuration so that a password was required:

```powershell
net user jsmith /passwordreq:yes
```

I verified the change and confirmed:

```text
PasswordRequired : True
```

---

## 🔑 Password Reset

I attempted to reset the password using PowerShell and encountered several syntax errors along the way.

Examples included:

```powershell
Ser-LocalUser
```

instead of:

```powershell
Set-LocalUser
```

and:

```powershell
Set -LocalUser
```

instead of:

```powershell
Set-LocalUser
```

These errors taught me how small syntax differences can completely change how PowerShell interprets a command.

To complete the reset cleanly, I used:

```powershell
net user jsmith *
```

Windows securely prompted for a new password and confirmed:

```text
The command completed successfully.
```

---

## ✅ Verification

I reviewed the account after the reset:

```powershell
net user jsmith
```

This confirmed:

```text
Account active       Yes
Password required    Yes
```

Finally, I tested the credentials by launching PowerShell as the `jsmith` account:

```powershell
runas /user:$env:COMPUTERNAME\jsmith powershell.exe
```

Windows returned:

```text
Attempting to start powershell.exe as user "COMPUTERNAME\jsmith" ...
```

✅ The new credentials were accepted successfully.

---

## 🧠 What I Actually Learned

This lab ended up teaching me more than a simple password reset.

* 🔹 How to create and inspect local Windows users
* 🔹 How to rename a local account
* 🔹 How to check whether an account is enabled
* 🔹 How to enforce password requirements
* 🔹 How to reset a password using multiple Windows tools
* 🔹 How to interpret PowerShell error messages
* 🔹 How tiny syntax mistakes affect cmdlet behavior
* 🔹 How to verify credentials after making a change

---

## 🛠️ Commands Used

```powershell
Get-LocalUser
New-LocalUser
Rename-LocalUser
Set-LocalUser
net user
runas
```

---

## 🎯 Biggest Takeaway

> **Troubleshooting is not about getting every command right the first time. It is about reading the error, understanding what happened, correcting it, and verifying the fix.**

---

## 🟢 Final Status

**Resolved**

The local user account was configured correctly, the password was successfully reset, and the new credentials were validated.
