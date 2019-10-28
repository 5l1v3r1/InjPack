# InjPack

_Tool Menu_

![](https://i.imgur.com/VYbDjDK.png)


_Default Injection Output_

![](https://i.imgur.com/OkA1wjx.png)



_Verbose Output Injection_

![](https://i.imgur.com/fYOCAXs.png)



_Target Program Not Running_

![](https://i.imgur.com/v6uLX2T.png)


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
If msbuild.exe isn't present in either of those directories, the tool will fail.