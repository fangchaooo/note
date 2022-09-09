# UE with VSCode

## Setting UE source code editor with vscode

1. change UE source code


```
// Engine/Config/Linux/LinuxEngine.ini
[/Script/SourceCodeAccess.SourceCodeAccessSettings]
PreferredAccessor=VisualStudioCodeSourceCodeAccess
```

2. change UE-Editor config

In UE Editor,Edit-Setting-General-SourceCode-Accessor-Visual Studio Code

3. change vscode path in UE plugin code

```cpp
//Engine/Plugins/Developer/VisualStudioCodeSourceCodeAccess/Source/VisualStudioCodeSourceCodeAccess/Private/VisualStudioCodeSourceCodeAccessor.cpp

#elif PLATFORM_LINUX
	FString OutURL;
	int32 ReturnCode = -1;
    
	FPlatformProcess::ExecProcess(TEXT("/bin/bash"), TEXT("-c \"type -p code\""), &ReturnCode, &OutURL, nullptr);
	if (ReturnCode == 0)
	{
		Location.URL = OutURL.TrimStartAndEnd();
	}
	else
	{
		// Fallback to default install location
		FString URL = TEXT(/* your vscode path in system */);
		if (FPaths::FileExists(URL))
		{
			Location.URL = URL;
		}
	}
```

Then, you can create a c++ UE project, you can find vscode starting, and your project reload in it.


## Build Debug UE C++ project with VScode

Above all, When you create UE C++ project, you can find project path tree in VScode. The important file path is `your project name/.vscode/tasks.json`

using `shift+crtl+b` to find task what you want to run.
