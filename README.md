# InjPack

_Tool Menu_

![](https://i.imgur.com/VYbDjDK.png)


_Default Injection Output_

![](https://i.imgur.com/OkA1wjx.png)



_Verbose Output Injection_

![](https://i.imgur.com/fYOCAXs.png)



_Target Program Not Running_

![](https://i.imgur.com/v6uLX2T.png)


## [Download ←](https://github.com/coltonon/InjPack/releases)
Extract the zip archive, then double click `InjPack.exe`.  
1. Type the target program name, ex (`starwarsbattlefrontii.exe`)
2. Select the path of the DLL you wish to pack
3. Type the DLL author name
4. Type the cheat name
5. Choose if you want manual map injection, or LoadLibrary.  Manual map is best most of the time, since the DLL doesn't have to be extracted.
6. Choose verbose output if you want it.  This'll dump a ton of useless crap to the console that nobodies ever going to read.  If this ain't checked, the injector will exit immediately upon success, so you'll basically just see a console flash real quick.
7.  Click merge.  If all goes well, after a minute you'll see a new explorer window pop open with your packed program selected.  If that doesn't happen, something screwed up.


## Why does this exist

When most people distribute a DLL, they'll simply upload the binary, then tell people to go download an injector.  This assumes the user knows what a DLL is, what an injector is, and how to use both.  From my experience, tons of people don't know how to do that.  There are a million DLL injectors, all with different settings and usages.  They can be confusing, overwhelming, and often times the user just doesn't care enough to fart around in one.  Additionally, sometimes the author wants specific dll injection methods, such as manual mapping, to be used; this puts more effor on the user.

I made this utility so I could lazily throw a DLL into it, click one button, then get a .exe that I can send to someone.  This .exe is an injector, with the DLL compiled into itself as a resource.  The injector has the target program name hardcoded into itself, as well as which injection technique to use.  This means all the user has to do is double click, and everything is taken care for them.

## Injection Techniques

* Manual Map
* LoadLibrary

Manual mapping is recommended if you don't want the DLL to be extracted to disk first.  The injector will stream the payload directly into the target process's memory.  LoadLibrary is nice for simplicity, and I just threw it in there to give you the option.

## Manual Extraction

If you've created a packed injector, or recieved one, simply pass the `extract` argument at the CLI to auto-extract the payload.

# WIP

I wrote this entire project in about an hour, there's little-to-no polish on anything.  The code is ugly, the output/logging is horrid, and its overall just hacked together.  Requires MSBuild to exist in one of the two paths:

```cs
string[] paths = { 
    @"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\MSBuild\15.0\Bin\",
    @"C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\"
};
```
If msbuild.exe isn't present in either of those directories, the tool will fail.  Also on VS 2017 every now and then the compiler would keel over and die if the resource was too big.  Not actually a problem with the code, but msbuild.  So far no problems with VS2019.

Basically this entire tool is just a VS solution, here's what the tool itself does:

1. Get user input for DLL, and game name
2. Write's those settings in `settings.h` (output will look something like:)

```c
#define PROGRAMNAME "starwarsbattlefrontii.exe"
#define DLLNAME "AnselHack.dll"
#define AUTHOR "Coltonon"
#define CHEATNAME "AnselHack"
#define VERBOSE false
#define MMAP true
```

3. Read the target DLL file, write it to `resource.cpp`
4. Call `MSBuild.exe` on the template, tell it to build in Release mode.
6. Copy the output file, open it in explorer