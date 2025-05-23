# [LYNIS](https://github.com/CISOfy/lynis)

One useful tool for auditing Unix-based systems (Linux, macOS, BDS, etc.) is [Lynis](https://github.com/CISOfy/lynis). This tool audits the current configuration of a system and provides additional hardening tips, taking into consideration various standards. It can be used by internal teams such as system administrators as well as third-parties (auditors and penetration testers) to obtain a "baseline" of the system's current security configuration. Again, this tool or others like it should not replace the manual techniques discussed in this module but can be a strong supplement to cover areas that may be overlooked.

After cloning the entire repo, we can run the tool by typing ./lynis audit system and receive a full report.

```bash
htb_student@NIX02:~$ ./lynis audit system

[ Lynis 3.0.1 ]

################################################################################
  Lynis comes with ABSOLUTELY NO WARRANTY. This is free software, and you are
  welcome to redistribute it under the terms of the GNU General Public License.
  See the LICENSE file for details about using this software.

  2007-2020, CISOfy - https://cisofy.com/lynis/
  Enterprise support available (compliance, plugins, interface and tools)
################################################################################


[+] Initializing program
------------------------------------

  ###################################################################
  #                                                                 #
  #   NON-PRIVILEGED SCAN MODE                                      #
  #                                                                 #
  ###################################################################

  NOTES:
  --------------
  * Some tests will be skipped (as they require root permissions)
  * Some tests might fail silently or give different results

  - Detecting OS...                                           [ DONE ]
  - Checking profiles...                                      [ DONE ]

  ---------------------------------------------------
  Program version:           3.0.1
  Operating system:          Linux
  Operating system name:     Ubuntu
  Operating system version:  16.04
  Kernel version:            4.4.0
  Hardware platform:         x86_64
  Hostname:                  NIX02

```
The resulting scan will be broken down into warnings:
```bash
Warnings (2):
  ----------------------------
  ! Found one or more cronjob files with incorrect file permissions (see log for details) [SCHD-7704] 
      https://cisofy.com/lynis/controls/SCHD-7704/

  ! systemd-timesyncd never successfully synchronized time [TIME-3185] 
      https://cisofy.com/lynis/controls/TIME-3185/
```
Suggestions:
```bash  
Suggestions (53):
  ----------------------------
  * Set a password on GRUB boot loader to prevent altering boot configuration (e.g. boot in single user mode without password) [BOOT-5122] 
      https://cisofy.com/lynis/controls/BOOT-5122/

  * If not required, consider explicit disabling of core dump in /etc/security/limits.conf file [KRNL-5820] 
      https://cisofy.com/lynis/controls/KRNL-5820/

  * Run pwck manually and correct any errors in the password file [AUTH-9228] 
      https://cisofy.com/lynis/controls/AUTH-9228/

  * Configure minimum encryption algorithm rounds in /etc/login.defs [AUTH-9230] 
      https://cisofy.com/lynis/controls/AUTH-9230/

```

and an overal scan details section:

```bash  
Lynis security scan details:

  Hardening index : 60 [############        ]
  Tests performed : 256
  Plugins enabled : 2

  Components:
  - Firewall               [X]
  - Malware scanner        [X]

  Scan mode:
  Normal [ ]  Forensics [ ]  Integration [ ]  Pentest [V] (running non-privileged)

  Lynis modules:
  - Compliance status      [?]
  - Security audit         [V]
  - Vulnerability scan     [V]

  Files:
  - Test and debug information      : /home/mrb3n/lynis.log
  - Report data                     : /home/mrb3n/lynis-report.dat
```