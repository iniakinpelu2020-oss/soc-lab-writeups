Setting Up Sysmon on Windows 10 and Reading Endpoint Logs

Summary
I installed Sysmon with the SwiftOnSecurity config on a Windows 10 machine and used Event Viewer to inspect process and network events.

Environment
- Windows 10 (Boot Camp on a MacBook Air), 8 GB RAM
- Sysmon v15.22
- SwiftOnSecurity sysmon-config

1. Normal baseline: Notepad (Event ID 1)
I opened Notepad and found its Process Create event.
- Image: C:\Windows\System32\notepad.exe
- ParentImage: C:\Windows\explorer.exe
- IntegrityLevel: Medium, run by my user account
- Signed Microsoft binary, so this is normal.

2. Flagged event: WMIADAP (Event ID 12 and 13)
Sysmon tagged registry events as "Suspicious,ImageBeginWithBackslash".
- Image: \\?\C:\Windows\system32\wbem\WMIADAP.EXE
- TargetObject: ...\Wbem\PROVIDERS\Performance\Performance Refreshed
- User: NT AUTHORITY\SYSTEM
- Conclusion: Windows performance counter refresh after I uninstalled software. Likely a false positive, because it is a built-in Windows tool in its normal folder.

3. Network event: OneDrive (Event ID 3)
- Image: OneDrive.exe in my user profile
- Connection initiated by my machine to port 443 (https)
- Conclusion: normal OneDrive sync with Microsoft.

Lessons learned
- A "Suspicious" tag is a reason to investigate, not a verdict.
- First checks: what program, who launched it, where it runs from, and where it connects.

## Next steps
Run Hayabusa on my event logs and write up the results.
