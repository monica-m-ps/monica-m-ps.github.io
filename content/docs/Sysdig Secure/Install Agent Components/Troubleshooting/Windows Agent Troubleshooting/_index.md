---
linkTitle: "Windows Agent Troubleshooting"
title: "Windows Agent Troubleshooting"
weight: "10"
aliases:
  - /en/windows-agent-troubleshooting.html
  - /en/windows-agent-troubleshooting
  - /en/docs/installation/sysdig-windows-agent/windows-agent-troubleshooting/
---

## Environments

### Hosts

Sysdig Windows agent consists of two Windows services:
1. Sysdig Connection Manager
2. Sysdig Security Manager

These services are configured to automatically restart when terminated. In case either of these services restarts frequently, a possible cause could be an Agent service crash. In order to troubleshoot such crashes, Microsoft Windows supports generating process memory dumps. The steps to enable program specific crash dumps can be found in [Collecting User-Mode Dumps](https://learn.microsoft.com/en-us/windows/win32/wer/collecting-user-mode-dumps). 

The agent logs for both the processes are available in the installation directory. This is typically `C:\Program Files\Sysdig\Agent\Logs`.

## Windows registry configuration for collecting agent crash dumps

Following configuration enables dumps for Sysdig Windows agent services with type `Full dump`. The dump file count per service is limited to `10`.

### Command Line

Please ensure the following commands are run as local administrator.
```cmd
reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps\connmgr.exe" /v DumpFolder /t REG_SZ /d "%PROGRAMFILES%\Sysdig\Agent\Dumps" /f

reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps\connmgr.exe" /v DumpType /t REG_DWORD /d 2 /f

reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps\connmgr.exe" /v DumpCount /t REG_DWORD /d 10 /f

reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps\secmgr.exe" /v DumpFolder /t REG_SZ /d "%PROGRAMFILES%\Sysdig\Agent\Dumps" /f

reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps\secmgr.exe" /v DumpType /t REG_DWORD /d 2 /f

reg.exe add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps\secmgr.exe" /v DumpCount /t REG_DWORD /d 10 /f
```

### Graphical UI
1. Launch Windows Registry Editor `regedit.exe` as administrator.

2. Navigate to the following registry key `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps`.

3. Right click on `LocalDumps` and select `New` -> `Key` and rename the new key to `connmgr.exe`.

4. Right click on `connmgr.exe` key and select `New` -> `String Value` and rename the new string value to `DumpFolder`.

5. Double click the `DumpFolder` value and set the value to `%PROGRAMFILES%\Sysdig\Agent\Dumps`.

6. Right click on `connmgr.exe` key and select `New` -> `DWORD (32-bit) Value` and rename the new DWORD value to `DumpType`.

7. Double click the `DumpType` value and set the value to `2`.

8. Right click on `connmgr.exe` key and select `New` -> `DWORD (32-bit) Value` and rename the new DWORD value to `DumpCount`.

9. Double click the `DumpCount` value and set the value to `10`.

Please repeat the above process for `secmgr.exe`.

### Dump Files

The generated dump file names are in the format of `{connmgr|secmgr}.exe.{pid}.dmp`.

