aria2c patched and optimized for Windows users.

<h3>Installation.</h3>

1. replace your old `aria2c.exe` with the one from either `patched_aria2-1.36.0-win-64bit-build1` or `patched_aria2-1.36.0-win-32bit-build1`.
 
2. copy the content of `additions/` to the same folder as `aria2c.exe`.

3. done.

<hr/>

<h3>Build information.</h3>

- same as: https://github.com/aria2/aria2/tree/release-1.36.0  

- https://github.com/aria2/aria2/releases/download/release-1.36.0/aria2-1.36.0-win-32bit-build1.zip  
and https://github.com/aria2/aria2/releases/download/release-1.36.0/aria2-1.36.0-win-64bit-build1.zip  
were binary-patched using tools from https://github.com/eladkarako/manifest and <a href="http://www.angusj.com/resourcehacker/">ResourceHacker</a>.


<h3>binary patches.</h3>

- embedded manifest (Windows 10 compatibility, high-DPI, long-path, segmented heap + side-by-side manifest).
- larger address awareness.
- icon.
- version info.

<h3>additional files.</h3>

- a `aria2c.conf` configuration file optimized for Windows users, working around Windows issues.  
- `aria2c.cmd` wrapping batch file.  

`aria2c.cmd` adds additional functionality:  
- loads `aria2c.conf` by default.  
- skips hard-coded checks for other configuration files.  
- local history file.  
- local last run log file.  
- fixes `check-certificate` being ignored.  
- enable Unicode support.

`aria2c.conf`'s content has some additional information.  

<hr/>

<h3>adding to <code>PATH</code></h3>

1. run `where aria2c` from Windows command-line.
2. the first line in the list is the one that Windows will use by default.
3. you can edit the system environment variables (including `PATH`) by running `systempropertiesadvanced.exe` (as admin), 
if more that one folder holds a binary file, the higher (or "first") the folder is will be favored over lowered ones.

<hr/>

Is it portable?
- yes. 

<hr/>

Optional extra steps:
edit scripts around your machine, do not use `aria2c.exe` explicitly, but just `aria2c`,  
this way Windows would (implicitly) favor batch-files such as `aria2c.cmd` (or `aria2c.bat`) when those are available, and fallback to just `aria2c.exe` when none-exists.

<hr/>

additional note:
- if you don't like the history or last log feature, just edit `aria2c.cmd`, remove the log argument and history call from it.

<hr/>
