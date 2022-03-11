

# windows-lua-luarocks-install-guide
Guide how to install lua and luarocks in window pc

The guide is copy of [awesome guide](https://web.archive.org/web/20210724103543/http://www.thijsschreijer.nl/blog/?p=863), with some small changes.

Guide is not available on site. So i make a copy. Use webarchive or this guide on github

What will this tutorial be doing;

 1. Install a compiler (MinGW (and MSYS))
 2. Compile and install Lua
 3. Install LuaRocks
 4. Install sone LuaRocks packages

# Installing the compiler

-   [Download MinGW](https://osdn.net/projects/mingw/). The ‘MinGW-get’ application allows you install MinGW components.
-   Start the installer and install the ‘mingw-get’ application (it’s windows caption is “MinGW Installation Manager”)
-   When installation completes, click ‘continue’ to select the required MinGW packages
-   Select the ‘basic setup’ on the left and select the following packages on the right;
    -   mingw32-base (Basic MinGW installation)
    -   mingw-gcc-g++ (The GNU C++ compiler)
    -   msys-base (A basic MSYS installation)
-   Now apply the changes (in menu “Installation”) and exit

As we will later be using the compiler, we need to make sure that the compiler can be found on the system. To do this, its location must be added to the system path. Here’s how;

-   right-click ‘my computer’ and select ‘properties’, click ‘advanced system settings’, select tab ‘advanced’, click ‘Environment variables…’
-   find the entry ‘PATH’ under ‘system variables’, and add ‘**c:\mingw\bin**’ (use a semicolon ‘;’ as a separator) to this variable.

We won’t be adding MSYS to the path, as it has some Unix like utilities that might interfere with the Windows tools.

# Installing Lua

-   [Download Lua source code](http://www.lua.org/ftp/). For this tutorial we’ll be using ‘lua-5.1.5.tar.gz’, but a 5.2 version should be easy to use as well.
-   Create a temporary folder ‘c:\temp\’ and store the downloaded file there. Open it and inside there will be a ‘lua-5.1.5’ folder, extract that folder into ‘c:\temp\lua-5.1.5\’.

Now if your system cannot handle ‘.tar.gz’ files (common on Unix, but not on Windows) you can [download 7zip](ttp://www.7-zip.org/), which is a compression utility that handles this format (and many others like .zip and .rar) very well.  
Compiling the Lua core files must be done from the command line. So;

-   open the start menu and type ‘cmd’, once ‘cmd.exe’ is found, open it.

**Before we can compile it we must temporarily add ‘MSYS’ to our system path to use some of it’s utilities, and we need to move into our Lua directory. To do so type the following commands;**
```
SET PATH=%PATH%;c:\mingw\msys\1.0\bin
CD c:\temp\lua-5.1.5
```

We’re all set to build our Lua installation, so type the following commands (in order; cleanup to make sure we start clean, then compile it using mingw, finally install the resulting binaries);
```
make clean
make mingw
make install INSTALL_TOP=c:/temp/lua/5.1 TO_BIN="lua.exe luac.exe lua51.dll"
```

**CRUCIAL** in the last line:

-   the version numbers 5.1 and 51 appear and must be correct (if using another version than 5.1)
-   ‘c:/temp/lua/5.1’ uses **FORWARD** slashes (a Unix convention)

 This should be your result:
```
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation. All rights reserved.
C:\windows\system32>SET PATH=%PATH%;c:\mingw\msys\1.0\bin

C:\windows\system32>CD c:\temp\lua-5.1.5

c:\temp\lua-5.1.5>make clean
cd src && make clean
make[1]: Entering directory `/c/temp/lua-5.1.5/src'
rm -f liblua.a lua luac lapi.o lcode.o ldebug.o ldo.o ldump.o lfunc.o lgc.o llex.o lmem.o lobject.o lopcodes.o lparser.o lstate.o lstring.o ltable.o ltm.o lundu
mp.o lvm.o lzio.o lauxlib.o lbaselib.o ldblib.o liolib.o lmathlib.o loslib.o ltablib.o lstrlib.o loadlib.o linit.o lua.o luac.o print.o
make[1]: Leaving directory `/c/temp/lua-5.1.5/src'

c:\temp\lua-5.1.5>make mingw
cd src && make mingw
make[1]: Entering directory `/c/temp/lua-5.1.5/src'
make "LUA_A=lua51.dll" "LUA_T=lua.exe" \
 "AR=gcc -shared -o" "RANLIB=strip --strip-unneeded" \
 "MYCFLAGS=-DLUA_BUILD_AS_DLL" "MYLIBS=" "MYLDFLAGS=-s" lua.exe
make[2]: Entering directory `/c/temp/lua-5.1.5/src'
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lua.o lua.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lapi.o lapi.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lcode.o lcode.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o ldebug.o ldebug.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o ldo.o ldo.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o ldump.o ldump.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lfunc.o lfunc.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lgc.o lgc.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o llex.o llex.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lmem.o lmem.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lobject.o lobject.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lopcodes.o lopcodes.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lparser.o lparser.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lstate.o lstate.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lstring.o lstring.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o ltable.o ltable.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o ltm.o ltm.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lundump.o lundump.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lvm.o lvm.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lzio.o lzio.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lauxlib.o lauxlib.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lbaselib.o lbaselib.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o ldblib.o ldblib.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o liolib.o liolib.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lmathlib.o lmathlib.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o loslib.o loslib.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o ltablib.o ltablib.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o lstrlib.o lstrlib.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o loadlib.o loadlib.c
gcc -O2 -Wall -DLUA_BUILD_AS_DLL -c -o linit.o linit.c
gcc -shared -o lua51.dll lapi.o lcode.o ldebug.o ldo.o ldump.o lfunc.o lgc.o llex.o lmem.o lobject.o lopcodes.o lparser.o lstate.o lstring.o ltable.o ltm.o lundump.o lvm.o lzio.o lauxlib.o lbaselib.o ldblib.o liolib.o lmathlib.o loslib.o ltablib.o lstrlib.o loadlib.o linit.o # DLL needs all object files
strip --strip-unneeded lua51.dll
gcc -o lua.exe -s lua.o lua51.dll -lm
make[2]: Leaving directory `/c/temp/lua-5.1.5/src'
make "LUAC_T=luac.exe" luac.exe
make[2]: Entering directory `/c/temp/lua-5.1.5/src'
gcc -O2 -Wall -c -o luac.o luac.c
gcc -O2 -Wall -c -o print.o print.c
ar rcu liblua.a lapi.o lcode.o ldebug.o ldo.o ldump.o lfunc.o lgc.o llex.o lmem.o lobject.o lopcodes.o lparser.o lstate.o lstring.o ltable.o ltm.o lundump.o lvm.o lzio.o lauxlib.o lbaselib.o ldblib.o liolib.o lmathlib.o loslib.o ltablib.o lstrlib.o loadlib.o linit.o # DLL needs all object files
ranlib liblua.a
gcc -o luac.exe luac.o print.o liblua.a -lm
make[2]: Leaving directory `/c/temp/lua-5.1.5/src'
make[1]: Leaving directory `/c/temp/lua-5.1.5/src'

c:\temp\lua-5.1.5>make install INSTALL_TOP=c:/temp/lua/5.1 TO_BIN="lua.exe luac.exe lua51.dll"
cd src && mkdir -p c:/temp/lua/5.1/bin c:/temp/lua/5.1/include c:/temp/lua/5.1/lib c:/temp/lua/5.1/man/man1 c:/temp/lua/5.1/share/lua/5.1 c:/temp/lua/5.1/lib/lua/5.1
cd src && install -p -m 0755 lua.exe luac.exe lua51.dll c:/temp/lua/5.1/bin
cd src && install -p -m 0644 lua.h luaconf.h lualib.h lauxlib.h ../etc/lua.hpp c:/temp/lua/5.1/include
cd src && install -p -m 0644 liblua.a c:/temp/lua/5.1/lib
cd doc && install -p -m 0644 lua.1 luac.1 c:/temp/lua/5.1/man/man1

c:\temp\lua-5.1.5>
```
Completing the setup

-   Now you can close the command window (important, you cannot re-use this instance!)
-   Open the ‘c:\temp’ folder in a Windows explorer window and you can move the complete ‘c:\temp\lua’ directory to ‘c:\program files\’ (or ‘c:\program files (x86)’ on 64bit systems)
-   Open the ‘PATH’ variable again in the ‘environment variables’ dialog box (see ‘installing the compiler’ above), this time adding the location of ‘lua.exe’ to the system path (that would be ‘c:\program files\lua\5.1\bin’ or include the ‘(x86)’ in case of a 64bit system).

Congratulations, you’ve just compiled and installed Lua! Open a command window and type;

lua -e "print('hello world')"

to test it (if it doesn’t work, make sure to open a new command window, because of the change to the PATH variable)

# Installing LuaRocks

-   [Download LuaRocks](http://luarocks.org/releases/).
- I use [luarocks-3.3.1-windows-32.zip](https://luarocks.github.io/luarocks/releases/luarocks-3.3.1-windows-32.zip) (luarocks.exe stand-alone Windows 32-bit binary)
- i recommend to use 32 version. I have problems with some libraries when try 64 bit.
-   Open the downloaded zip file and drag the ‘luarocks-3.3.1-windows-32’ folder inside to  ‘c:\program files\’ 
- Add to PATH 'C:\program files\luarocks-3.3.1-windows-32'
- Open cmd and type:'luarocks --lua-version 5.1 path'
you will see something like that
```
SET LUA_PATH=C:\develop\luarocks-3.3.1-windows-32\lua\?.lua;C:\develop\luarocks-3.3.1-windows-32\lua\?\init.lua;C:\develop\luarocks-3.3.1-windows-32\?.lua;C:\develop\luarocks-3.3.1-windows-32\?\init.lua;C:\develop\luarocks-3.3.1-windows-32\..\share\lua\5.3\?.lua;C:\develop\luarocks-3.3.1-windows-32\..\share\lua\5.3\?\init.lua;.\?.lua;.\?\init.lua;C:\Users\d.popov\AppData\Roaming/luarocks/share/lua/5.1/?.lua;C:\Users\d.popov\AppData\Roaming/luarocks/share/lua/5.1/?/init.lua 
SET LUA_CPATH=C:\develop\luarocks-3.3.1-windows-32\?.dll;C:\develop\luarocks-3.3.1-windows-32\..\lib\lua\5.3\?.dll;C:\develop\luarocks-3.3.1-windows-32\loadall.dll;.\?.dll;C:\Users\d.popov\AppData\Roaming/luarocks/lib/lua/5.1/?.dll 
SET PATH=C:\Users\d.popov\AppData\Roaming/luarocks/bin;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\Program Files\Intel\WiFi\bin\;C:\Program Files\Common Files\Intel\WirelessCommon\;C:\Program Files\Git\cmd;C:\MinGW\bin;C:\develop\lua\5.1\bin;C:\develop\luarocks-3.3.1-windows-32;;C:\HaxeToolkit\haxe;C:\HaxeToolkit\neko;C:\Program Files\nodejs\;C:\Users\d.popov\AppData\Local\Microsoft\WindowsApps;C:\Users\d.popov\AppData\Roaming\npm
```
-   Open the window with the ‘environment variables’ again and add the values given to the variables there (only if they are not there already). LUA_PATH and LUA_CPATH (or _5_2 versions) probably do not exist, so you’ll have to create them. This step is crucial for your Lua installation to work properly, so be carefull to get this right!
- Also not existed values to PATH. Example:'C:\Users\user\AppData\Roaming/luarocks/bin;'

Now close all command windows and open a new one and type
```luarocks help```
to see whether your new LuaRocks installation is found.

# Installing LuaRocks packages

For example you can install some packages. 
You need to add --lua-version 5.1 to your command.
```
luarocks --lua-version 5.1 install lua-cjson
luarocks --lua-version 5.1 install luautf8
luarocks --lua-version 5.1 install luasocket
luarocks --lua-version 5.1 install bit32
luarocks --lua-version 5.1 install luabitop
```
Congratulations again! you just setup a working package manager. This will allow you to install Lua modules with just a few simple commands, while not having to worry about the technical complexities of the compiler. Obviously only packages intended for Windows can be installed this way, explicit unix packages will not work.
