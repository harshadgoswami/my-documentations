This usually happens because the Windows OpenSSH Authentication Agent service is disabled, missing, or PowerShell isn't running with administrator privileges.

Let's check which situation applies on your PC.

## 1. Open PowerShell as Administrator

1. Click the Windows Start menu.

2. Search for `PowerShell`.

3. Right-click Windows PowerShell and select Run as administrator.

4. Run:

PowerShell

```
Get-Service ssh-agent
```

## 2. Depending on the error, try the matching fix

## A. If you see `Access is denied`

Make sure PowerShell is running as Administrator, then execute:

PowerShell

```
Set-Service ssh-agent -StartupType Automatic
Start-Service ssh-agent
```

## B. If you see `Cannot find any service with service name 'ssh-agent'`

The Windows OpenSSH Client may not be installed.

Open Settings → System → Optional features → View features and search for `OpenSSH Client`. Install it if it's missing.

Then restart PowerShell as Administrator and check:

PowerShell

```
Get-Service ssh-agent
```

## C. If you see `The service cannot be started`

Check the service status and startup type:

PowerShell

```
Get-Service ssh-agent | Format-List *
```

If the service is present, try:

PowerShell

```
Set-Service ssh-agent -StartupType Manual
Start-Service ssh-agent
```

## 3. Alternative: use Git Bash without the Windows service

If you have Git for Windows installed, open Git Bash and run:

Bash

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
```

This starts a temporary SSH agent in that Git Bash session. You may need to repeat the commands when you open a new session.

## 4. One thing I need from you

Please copy and paste the exact error message you get when running the original commands. For example, does it say `Access is denied`, `Cannot find any service`, or something else? That will tell us which fix to use.
