# Bypass screensaver with vbscript on Windows 8.1

## Win8.1 did not allow:
- run vbs script directly from registry
	- HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
	- HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
	- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run
	- HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run
	- HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
	- HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\RunOnce
- run 32bit vbs script from bat from registry
- run powershell script from registry
	- powershell call vbscript
	- powershell move mouse only
	
```ps1
Add-Type -AssemblyName System.Windows.Forms 
 
    while(1) {  
    $position = [System.Windows.Forms.Cursor]::Position  
    $position.X++  	
	[System.Windows.Forms.Cursor]::Position = $position  
	
    #$time = Get-Date;  
    #$shorterTimeString = $time.ToString("HH:mm:ss");  

    #Write-Host $shorterTimeString "Mouse pointer has been moved 1 pixel to the right"  
    #Set your duration between each mouse move
    Start-Sleep -Seconds 50

	[System.Windows.Forms.Cursor]::Position = $position  
    $position.X--  
    [System.Windows.Forms.Cursor]::Position = $position  
    }  
```

- run vbs script from startup folder directly


## Solution to Add Startup Programs for a "Specific User" Only:
1. You could also press **Windows+R** to open Run
2. Type **shell:Startup**, and click/tap on OK.
	- C:\Users\**(User-Name)**\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
3. Copy bat file to current user's startup folder that call vbscript. After windows start it will be run as foreground command.

bypass_screensaver_vbs.bat file:

```bat
%windir%\SysWoW64\cmd.exe /c cscript C:\bypass_screensaver.vbs
```

bypass_screensaver.vbs file:

```csharp
Set ws = CreateObject("WScript.Shell") 
Do 
    Wscript.Sleep 50000 
    ws.SendKeys "{F15}" 
Loop
```

F15 is valid key (SHIFT + F3) but nothing use it.

> Other startup methods [here](https://www.eightforums.com/tutorials/5180-startup-items-manage-windows-8-a.html).