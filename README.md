binary patched `aria2c`.  

this is mostly just for me,  
but feel free to use it as well.  

sources:  

<details><summary>

`patched_official_aria2-1.36.0-win-32bit-build1` and `patched_official_aria2-1.36.0-win-64bit-build1` are based on https://github.com/aria2/aria2/releases/tag/release-1.36.0  

</summary>

```txt
Enabled Features: Async DNS, BitTorrent, Firefox3 Cookie, GZip, HTTPS, Message Digest, Metalink, XML-RPC, SFTP
Hash Algorithms: sha-1, sha-224, sha-256, sha-384, sha-512, md5, adler32
Libraries: zlib/1.2.11 expat/2.4.1 sqlite3/3.36.0 GMP/6.2.1 c-ares/1.17.2 libssh2/1.9.0
Compiler: mingw-w64 8.0.0 (alpha) / gcc 10-win32 20210110
  built by  x86_64-pc-linux-gnu

32bit
  targeting i686-w64-mingw32
  on        Aug 21 2021 17:33:51
System: Windows Vista (10.0)

64bit
  targeting x86_64-w64-mingw32
  on        Aug 21 2021 17:37:16
System: Windows Vista (x86_64) (10.0)
```

</details>

<details><summary>

`patched_fork_aria2-2048-threads_aria2-1.36.0-win-32bit-build1` and `patched_fork_aria2-2048-threads_aria2-1.36.0-win-64bit-build1` are based on https://github.com/chitao1234/aria2-2048-threads/releases/tag/snapshot-20220625

</summary>

```txt
Enabled Features: Async DNS, BitTorrent, Firefox3 Cookie, GZip, HTTPS, Message Digest, Metalink, XML-RPC, SFTP
Hash Algorithms: sha-1, sha-224, sha-256, sha-384, sha-512, md5, adler32
Libraries: zlib/1.2.12 expat/2.4.8 sqlite3/3.38.5 GMP/6.2.1 c-ares/1.18.1 libssh2/1.10.0
Compiler: mingw-w64 8.0.0 (alpha) / gcc 10-win32 20220113

32bit
  built by  x86_64-pc-linux-gnu
  targeting i686-w64-mingw32
  on        Jun 24 2022 09:36:07
System: Windows Vista (10.0)

64bit
  built by  x86_64-pc-linux-gnu
  targeting x86_64-w64-mingw32
  on        Jun 24 2022 10:10:28
System: Windows Vista (x86_64) (10.0)
```

</details>


0. download https://github.com/eladkarako/aria2c_win/archive/refs/heads/master.zip
1. get x64 version (official or "2048-threads").
2. replace your old `aria2c.exe` with this one.
3. optional - copy all files from `additions` to the same folder.
4. optional - download https://curl.se/ca/cacert.pem to be used with `--ca-certificate=`.
5. optional - add the folder to the system `PATH` to run `aria2c` from everywhere. you can run `where aria2c` to figure out if it is already in the system `PATH`.

- "2048-thread" uses a very small value of `--min-split-size` by default, it sometimes causes websites to send incorrect partial-content (HTTP 206), you are advised to normalize it with `--min-split-size=5M`.
- normally you'll be limited to `--max-connection-per-server=16` and `--max-concurrent-downloads=32` as the maximum values, but with "2048-thread" you can set it to larger values.  

Windows normally won't let you open too much connections as a protection in case your computer have turned into a bot by a worm or a malicious software. you can override it, or revert it back, using registry modifications.

to revert:  

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\Tcpip\Parameters]
"TcpNumConnections"=-
"TcpMaxHalfOpen"=-
"TcpMaxHalfOpenRetried"=-
"TcpMaxPortsExhausted"=-
"SynAttackProtect"=-

;TcpNumConnections
;   maximum number of connections that TCP can have open simultaneously.
;   default:     not there (not "0"!!!)
;   limits:      0-16777214  or  0-FFFFFE
;   good value:  500         or  1F4
;   https://technet.microsoft.com/en-us/library/cc938216.aspx

;TcpMaxHalfOpen
;   maintain in the half-open (SYN-RCVD) state before TCP/IP initiates SYN flooding attack protection 
;   default:    Windows NT Server: 0x64 ( 100 ) Windows NT Workstation: 0x1F4 ( 500 )
;   limits:     0x64–0xFFFF ( connections )     100 or 65535
;   good value: 1500 or 5DC
;   https://technet.microsoft.com/en-us/library/cc938212.aspx

;TcpMaxHalfOpenRetried
;   limit        0x50 ( 80 )–0xFFFF
;   default      Windows NT Server: 0x50 ( 80 ) Windows NT Workstation: 0x190 ( 400 )
;   good value:  1000 or 3E8
;   https://technet.microsoft.com/en-us/library/cc938213.aspx

;TcpMaxPortsExhausted
;   limits:      0-65535  or  0x0–0xFFFF
;   default:     5        or  0x5
;   good value:  2000     or  7D0
;   https://technet.microsoft.com/en-us/library/cc938214.aspx

;SynAttackProtect
;   default 0
;   should be kept 0
;   https://technet.microsoft.com/en-us/library/cc938202.aspx
```

this is quite suitable:  

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\Tcpip\Parameters]
"TcpNumConnections"=dword:000001f4
"TcpMaxHalfOpen"=dword:000005dc
"TcpMaxHalfOpenRetried"=dword:000003e8
"TcpMaxPortsExhausted"=dword:000007d0
"SynAttackProtect"=dword:00000000
```

this is extreme, useful for file-sharing:  

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\Tcpip\Parameters]
"TcpNumConnections"=dword:000007d0
"TcpMaxHalfOpen"=dword:00000fa0
"TcpMaxHalfOpenRetried"=dword:00000bb8
"TcpMaxPortsExhausted"=dword:00001388
"SynAttackProtect"=dword:00000000
```

<hr/>

using `aria2c.cmd` instead of `aria2c.exe`,  
or simply calling `aria2c` from that folder will call implicitly to `aria2c.cmd` that's how Windows works.  
when you run `where aria2c` (and the folder where the files are placed is in the system `PATH`) you should see first `aria2c.cmd` then `aria2c.exe`, again this is how Windows works. a generic call to `aria2c` will execute the first path you've got in the `where aria`, if you see path to other version first, this is the one that would be launched.. to change it you have to edit your system `PATH`, putting the desired folder before the other one ("first come, first serve"), changes would require closing command-lines and re-opening them, or sometime - a reboot. Google how to change the system `PATH` just in case..

`aria2c.cmd` includes various fixes and normalizations for many quirks `aria2` have with Windows,  
it also by default runs with `--check-certificate=false`.  
the reason is Windows does not handle well a CA from file, since it uses its own certificate storage.  
`--ca-certificate=` might still works for you, but a lot of users are getting authentication errors,  
this is the main reason I've even started this whole thing..

all modifications to `aria2c.exe` files any binary,  
the code is the same as the the release links specified above.  
- adding icon.
- adding version information.
- adding compatible manifest.
- enabling extended memory allocation for x32bit exe.
- the manifest makes the exe run not through the vista virtualization engine, so faster.
- the manifest allow the exe to use newly efficient segmented heap.
- the manifest allows long file path support, but you also need a registy patch to activate it in Windows. Google it.

<hr/>

this is a batch-file that resets and cleans and solves some connection problems:  

```cmd
@echo on
::-------------------------------------------------------------------Firewall Reset (firewall works on XP,7,8+, advfirewall work on 7,8+)
netsh firewall reset
netsh advfirewall reset

::-------------------------------------------------------------------Disable Firewall (firewall works on XP,7,8+, advfirewall work on 7,8+)
netsh firewall set opmode mode=DISABLE profile=ALL
netsh advfirewall set allprofiles state off

::-------------------------------------------------------------------delete http cache
netsh nap reset
netsh rpc reset
netsh winhttp reset
netsh http flush
netsh http delete timeout timeouttype=idleconnectiontimeout
netsh http delete timeout timeouttype=headerwaittimeout

::-------------------------------------------------------------------make connection direct
netsh winhttp reset proxy

::-------------------------------------------------------------------disable tracing (default = disabled, ansi, 65535)
netsh winhttp reset tracing

::-------------------------------------------------------------------delete http cache

netsh http delete cache

::-------------------------------------------------------------------BranchCache Optimize WAN traffic (Windows Server 2008 R2 and Windows® 7)
netsh branchcache reset

::-------------------------------------------------------------------Routing Lists Clear
netsh routing reset

::-------------------------------------------------------------------Network-Adapter’s Software Default (Winsock Reset and Rebuild)
netsh winsock reset

::-------------------------------------------------------------------BranchCache is a new feature of Windows Server 2008 R2 and Windows® 7. BranchCache 
netsh interface ipv4 reset
netsh interface ipv6 reset

::-------------------------------------------------------------------Network-Interfaces Reset
netsh interface reset all

netsh interface httpstunnel reset


::-------------------------------------------------------------------Hardcore TCP/IP Reset and Rebuild
netsh int ip reset c:\temp\netsh_ip_reset_log.txt



pause
```

after that re-run the reg-file,  
and reset the machine.
