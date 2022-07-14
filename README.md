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
1. get x64 version of the 2048-threads.
2. replace your old `aria2c.exe` with this one.
3. optional - copy all files from `additions` to the same folder.
4. optional - download https://curl.se/ca/cacert.pem to be used with `--ca-certificate=`.
5. optional - add the folder to the system `PATH` to run `aria2c` from everywhere. you can run `where aria2c` to figure out if it is already in the system `PATH`.


<details><summary>notes</summary>

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

</details>

