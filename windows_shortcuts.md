---
title: Atalhos Windows
---

Alguns comandos úteis para acesso rápido ou para uso em scripts/automação no Windows.

------

Add Network Location (wizard)

    rundll32.exe shwebsvc.dll,AddNetPlaceRunDll

Add-Remove Programs

    rundll32.exe shell32.dll,Control_RunDLL appwiz.cpl

Add-Remove Windows Components

    rundll32.exe shell32.dll,Control_RunDLL appwiz.cpl,,2

Advanced Restore

    sdclt.exe /restorewizardadmin

Aero (Transparency) Off

    rundll32.exe DwmApi #104

Aero (Transparency) On

    rundll32.exe DwmApi #102

Backup Location & Settings

    sdclt.exe /configure

Control Panel

    rundll32.exe shell32.dll,Control_RunDLL

Date and Time Additional Clocks

    rundll32.exe shell32.dll,Control_RunDLL timedate.cpl,,1

Date and Time Properties

    rundll32.exe shell32.dll,Control_RunDLL timedate.cpl

Device Manager

    rundll32.exe devmgr.dll DeviceManager_Execute

Device Manager

    rundll32.exe shell32.dll,Control_RunDLL hdwwiz.cpl

Exit from windows with no messages.

    rundll32.exe krnl386.exe,exitkernel

Folder Options – General

    rundll32.exe shell32.dll,Options_RunDLL 0

Folder Options – Search

    rundll32.exe shell32.dll,Options_RunDLL 2

Folder Options – View

    rundll32.exe shell32.dll,Options_RunDLL 7

Forgotten Password Wizard

    rundll32.exe keymgr.dll,PRShowSaveWizardExW

Hibernate

    rundll32.exe powrprof.dll,SetSuspendState

Indexing Options

    control.exe srchadmin.dll

Internet Explorer - Delete All

    rundll32.exe InetCpl.cpl,ClearMyTracksByProcess 255

Internet Explorer - Delete All + files and settings stored by Add-ons:

    rundll32.exe InetCpl.cpl,ClearMyTracksByProcess 4351

Internet Explorer - Delete Cookies

    rundll32.exe InetCpl.cpl,ClearMyTracksByProcess 2

Internet Explorer - Delete Form Data

    rundll32.exe InetCpl.cpl,ClearMyTracksByProcess 16

Internet Explorer - Delete History

    rundll32.exe InetCpl.cpl,ClearMyTracksByProcess 1

Internet Explorer - Delete Passwords

    rundll32.exe InetCpl.cpl,ClearMyTracksByProcess 32

Internet Explorer - Delete Temporary Internet Files

    rundll32.exe InetCpl.cpl,ClearMyTracksByProcess 8

Internet Explorer’s Internet Properties dialog box.

    rundll32.exe shell32.dll,ConBring up trol_RunDLL Inetcpl.cpl,,6

Keyboard Properties

    rundll32.exe shell32.dll,Control_RunDLL main.cpl @1

Lock Screen

    rundll32.exe user32.dll,LockWorkStation

Map Network Drive Wizard

    rundll32.exe shell32.dll,SHHelpShortcuts_RunDLL Connect

Mouse Button – Swap left button to function as right

    rundll32.exe user32.dll,SwapMouseButton

Mouse Properties

    rundll32.exe shell32.dll,Control_RunDLL main.cpl @0

Network Connections

    rundll32.exe shell32.dll,Control_RunDLL ncpa.cpl

Notification Cache

    rundll32.exe shell32.dll,Options_RunDLL 5

Open Control Panel (All Items)

    explorer.exe shell:::{21ec2020-3aea-1069-a2dd-08002b30309d}

Open control panel.

    rundll32.exe shell32,Control_RunDLL

Open screen properties.

    rundll32.exe shell32,Control_RunDLL desk.cpl

Open window - About – Windows

    rundll32.exe shell32,ShellAboutA Info-Box

Open window - Open with

    rundll32.exe shell32,OpenAs_RunDLL

Open With Dialog Box

    rundll32.exe shell32.dll,OpenAs_RunDLL Any_File-name.ext

Internet Explorer - Organize Favourites

    rundll32.exe shdocvw.dll,DoOrganizeFavDlg

Pen and Touch Tablet PC Settings

    rundll32.exe shell32.dll,Control_RunDLL tabletpc.cpl

Pen and Touch Tablet PC Settings Flicks Tab

    rundll32.exe shell32.dll,Control_RunDLL tabletpc.cpl,,1

Pen and Touch Tablet PC Settings Handwriting Tab

    rundll32.exe shell32.dll,Control_RunDLL tabletpc.cpl,,2

People Near Me

    rundll32.exe shell32.dll,Control_RunDLL collab.cpl

Personalization

    rundll32.exe shell32.dll,Control_RunDLL desk.cpl,,2

Phone and Modem Options

    rundll32.exe shell32.dll,Control_RunDLL telephon.cpl

Phone and Modem Options Modems Tab

    rundll32.exe shell32.dll,Control_RunDLL telephon.cpl,,1

Phone and Modems Options Advanced Tab

    rundll32.exe shell32.dll,Control_RunDLL telephon.cpl,,2

Power Options

    rundll32.exe shell32.dll,Control_RunDLL powercfg.cpl

Power Options Change Plan Settings

    rundll32.exe shell32.dll,Control_RunDLL powercfg.cpl,,1

Printer Management Folder.

    rundll32.exe shell32.dll,SHHelpShortcuts_RunDLL PrintersFolder

Printer User Interface

    rundll32.exe Printui.dll,PrintUIEntry /?

Process Idle Tasks

    rundll32.exe advapi32.dll,ProcessIdleTasks

Reboot computer.

    rundll32.exe shell32,SHExitWindowsEx 2

Regional and Language Options

    rundll32.exe shell32.dll,Control_RunDLL Intl.cpl,,0

Regional Settings

    rundll32.exe shell32.dll,Control_RunDLL intl.cpl,,3

Restart Explorer.

    rundll32.exe shell32,SHExitWindowsEx -1

Restart Windows 98 without 

    rundll32.exe shell32,SHExitWindowsEx 0autoexec.bat etc.

Restore Files

    sdclt.exe /restorewizard

Safely Remove Hardware Dialog Box

    rundll32.exe shell32.dll,Control_RunDLL HotPlug.dll

Screen Resolution

    rundll32.exe shell32.dll,Control_RunDLL desk.cpl

Screen Saver

    rundll32.exe shell32.dll,Control_RunDLL desk.cpl,,1

Set Program Access and Computer Defaults

    rundll32.exe shell32.dll,Control_RunDLL appwiz.cpl,,3

Shutdown computer.

    rundll32.exe shell32,SHExitWindowsEx 1

Sound Properties Dialog Box

    rundll32.exe shell32.dll,Control_RunDLL Mmsys.cpl,,0

Stored Usernames and Passwords

    rundll32.exe keymgr.dll,KRShowKeyMgr

System Properties

    rundll32.exe shell32.dll,Control_RunDLL sysdm.cpl

System Properties Advanced Tab

    rundll32.exe shell32.dll,Control_RunDLL sysdm.cpl,,3

System Properties Hardware Tab

    rundll32.exe shell32.dll,Control_RunDLL sysdm.cpl,,2

System Properties System Protection Tab

    rundll32.exe shell32.dll,Control_RunDLL sysdm.cpl,,4

System Properties: Automatic Updates

    rundll32.exe shell32.dll,Control_RunDLL sysdm.cpl,,5

Taskbar Properties

    rundll32.exe shell32.dll,Options_RunDLL 1

User Accounts

    rundll32.exe shell32.dll,Control_RunDLL nusrmgr.cpl

Windows – About

    rundll32.exe shell32.DLL,ShellAboutW

Windows Firewall

    rundll32.exe shell32.dll,Control_RunDLL firewall.cpl

Windows Fonts Installation Folder

    rundll32.exe shell32.dll,SHHelpShortcuts_RunDLL FontsFolder

Windows Security Center

    rundll32.exe shell32.dll,Control_RunDLL wscui.cpl

