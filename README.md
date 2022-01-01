# Run Command as User in PowerShell

Want to run a command as a specific user in PowerShell without having to type in the password?

First, save the password of the user as a secure string by typing this in PowerShell:

    Read-Host -AsSecureString | ConvertFrom-Securestring | Out-File C:\UserCred.txt -Force

Just type the password on the blank line and press enter.

Once it's saved, you can use these commands to launch the process. Make sure to adjust the username.

    $Password = Get-Content C:\UserCred.txt | ConvertTo-SecureString
    $StartProcInfo = New-Object System.Diagnostics.ProcessStartInfo
    $StartProcInfo.UserName = 'User'
    $StartProcInfo.Password = $Password
    $StartProcInfo.UseShellExecute =  $false
    $StartProcInfo.FileName = "powershell"
    $StartProcInfo.Arguments = "-NoExit"
    [System.Diagnostics.Process]::Start($StartProcInfo)

# As a PowerShell Function
If you need to do this often, add a function in your PowerShell profile by typing:

`code $profile`

Then add a new function - make sure to adjust the username:

```
function RunAsUser {
	param($Path,$Arguments)
	$password = Get-Content C:\UserCred.txt | ConvertTo-SecureString
	$StartProcInfo = New-Object System.Diagnostics.ProcessStartInfo
	$StartProcInfo.UserName = 'User'
	$StartProcInfo.Password = $Password
	$StartProcInfo.UseShellExecute = $false
	$StartProcInfo.FileName = $Path
	$StartProcInfo.Arguments = $Arguments
	[System.Diagnostics.Process]::Start($StartProcInfo)
}
```

Save it, then close PowerShell and reopen.

Now you can simply type this in the future:
`RunAsUser -Path powershell -Arguments "-NoExit -Command whoami"`

Adjust path of the password file or other options as needed.

Result:

![result](https://i.imgur.com/9cAOlxS.png)
