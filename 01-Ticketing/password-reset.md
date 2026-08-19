Ticket #001 - User Password Reset

Ticket Summary

Issue: User is unable to log in to their Windows account.

Priority: Medium

Category: Account / Authentication

Status: In Progress

User Report

The user reports that they are unable to log in to their Windows workstation and believes they may have forgotten their password.

Initial Troubleshooting Plan
Confirm the username being used.
Verify that Caps Lock and Num Lock are not causing incorrect password entry.
Determine whether the account is locked or the password is incorrect.
Reset the user's password if necessary.
Verify that the user can successfully log in after the reset.
Document the resolution.
Skills Practiced
User account troubleshooting
Windows authentication troubleshooting
Password reset procedures
Help desk ticket documentation
Basic security best practices
Resolution

The user's local Windows account was verified using PowerShell to confirm that the account existed and was enabled.

I checked the account configuration using:

Get-LocalUser

and verified additional properties including account status, password requirements, password expiration, and last logon information.

During troubleshooting, I discovered that the test account did not initially require a password. I corrected the account configuration so that password authentication was required.

The user's password was then reset using administrative credentials. I verified that the account remained enabled and that a password was required.

To confirm the resolution, I tested authentication using:

runas /user:$env:COMPUTERNAME\jsmith powershell.exe

After entering the newly assigned temporary password, Windows successfully launched a process under the jsmith account.

I verified the authenticated user context using:

whoami

Final Status: Resolved ✅

Commands Used
-Get-LocalUser
-Get-LocalUser -Name "jsmith"
-Set-LocalUser -Name "jsmith" -Password $tempPassword
-net user jsmith /passwordreq:yes
-net user jsmith *
-runas /user:$env:COMPUTERNAME\jsmith powershell.exe
-whoami

Lessons Learned 

-Learned how to create and manage local Windows user accounts through PowerShell.
-Practiced checking whether an account is enabled and whether password authentication is required.
-Learned how PowerShell syntax errors can change how a command is interpreted.
-Practiced reading error messages and correcting commands rather than immediately starting over.
-Learned multiple methods for resetting a local Windows user's password.
-Used runas to validate credentials after a password reset.
-Used whoami to verify the security context of the authenticated user.
-Reinforced the importance of verifying that a fix actually works before closing a help desk ticket.
