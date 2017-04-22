# Bypass screensaver with vbscript on Windows 8.1

## Win8.1 did not allow:
-run vbs script directly from registry (HKCU,HKLM, Run)
-run 32bit vbs script from bat from registry (HKCU,HKLM, Run)
-run powershell script from registry  (HKCU,HKLM, Run)
	-powershell call vbscript
	-powershell move mouse only
-run vbs script from startup folder directly


## Solution:
1. Push **Windows+R**
2. Run **shell:startup**
3. copy bat file to current user's startup folder that call vbscript. After windows start it will be run as foreground command.

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