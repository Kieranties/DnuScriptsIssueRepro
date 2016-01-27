Issue: Dependant projects do not have their 'scripts' executed
==============================================================

Setup
-----

+ The project *WithScripts* contains an entry for each custom script available in a project.json.
+ The project *DnuScriptsIssueRepro* has a dependency on the *WithScripts* project.

Issue
-----

When running `dnu build` from the *WithScripts* folder the following is displayed 

```
C:\Code\Git\DnuScriptsIssueRepro\src\WithScripts
> dnu build
Microsoft .NET Development Utility Clr-x86-1.0.0-rc2-16389


Building WithScripts for .NETFramework,Version=v4.5.1
Executing script 'prebuild' in project.json
PRE-BUILD
  Using Project dependency WithScripts 1.0.0-local
    Source: C:\Code\Git\DnuScriptsIssueRepro\src\WithScripts\project.json

    // snip dependencies

Executing script 'postbuild' in project.json
POST-BUILD

  // snip repeated info for multiple platforms
 ```
 
 **NOTE**: in the above the pre and post build tasks are executed successfully
 
 When running `dnu build` from the *DnuScriptsIssueRepro* folder the following is displayed
 
 ```
 C:\Code\Git\DnuScriptsIssueRepro\src\DnuScriptsIssueRepro [master = +1 ~0 -0 !]
> dnu build
Microsoft .NET Development Utility Clr-x86-1.0.0-rc2-16389


Building DnuScriptsIssueRepro for .NETFramework,Version=v4.5.1
  Using Project dependency DnuScriptsIssueRepro 1.0.0-local
    Source: C:\Code\Git\DnuScriptsIssueRepro\src\DnuScriptsIssueRepro\project.json

  Using Project dependency WithScripts 1.0.0-local
    Source: C:\Code\Git\DnuScriptsIssueRepro\src\WithScripts\project.json

  Using Assembly dependency fx/mscorlib 4.0.0
    Source: C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5.1\mscorlib.dll

  Using Assembly dependency fx/System 4.0.0
    Source: C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5.1\System.dll

  Using Assembly dependency fx/System.Core 4.0.0
    Source: C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5.1\System.Core.dll

  Using Assembly dependency fx/Microsoft.CSharp 4.0.0
    Source: C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5.1\Microsoft.CSharp.
    
     // snip repeated info for multiple platforms
```

**NOTE**: The pre and post build tasks for the *WithScripts* project are **not** executed, even though its build output is a dependency.

Expected Result
---------------

It is expected that when running `dnu build` for *DnuScriptsIssueRepro* that the build tasks for its dependant projects are also executed.

In my real world scenario, I have resources that are generated during prebuild of a project (Project A).
when Project B references Project A and the build command is called on Project B, the resources are not compiled into Project A.

Thus Project A contains none of the expected resources and any tests fail.