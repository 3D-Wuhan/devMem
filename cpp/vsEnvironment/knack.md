# knack

[Devenv command-line switches](https://docs.microsoft.com/en-us/visualstudio/ide/reference/devenv-command-line-switches?view=vs-2019)

[Synchronize Visual Studio settings across multiple computers](https://docs.microsoft.com/en-us/visualstudio/ide/synchronized-settings-in-visual-studio?view=vs-2019)

[How do I REALLY reset the Visual Studio window layout?](https://stackoverflow.com/questions/26863/how-do-i-really-reset-the-visual-studio-window-layout)

<https://docs.microsoft.com/zh-cn/cpp/build/building-on-the-command-line?view=msvc-160>

----------------------

```shell
cmake .

msbuild .\hello_world.sln
```

## 1 devenv

```shell
devenv /SafeMode
```

Starts Visual Studio in safe mode, loading only the default environment and services.
This switch prevents all third-party VSPackages from loading when Visual Studio 
starts, allowing stable execution.

```shell
devenv /ResetSettings [SettingsFile|DefaultCollectionSpecifier]
```

Arguments
* *SettingsFile*

Optional. The full path and name of the settings file to apply to Visual Studio.

* *DefaultCollectionSpecifier*

Optional. A specifier representing a default collection of settings to restore. 
Choose one of the default collection specifiers listed in the table.

| Default collection name | Collection specifier |
| --- | --- |
| General | General |
| JavaScript | JavaScript |
| Visual Basic | VB |
| Visual C# | CSharp |
| Visual C++ | VC |
| Web Development | Web |
| Web Development | (Code Only)	WebCode |

**Remarks**

If no SettingsFile is specified, the IDE opens using the existing settings.

**Example**

The first example applies the settings stored in the file MySettings.vssettings.

The second example restores the Visual C# default profile.

```shell
devenv /resetsettings "%USERPROFILE%\MySettings.vssettings"

devenv /resetsettings CSharp
```

## 2 Import and Export VS Settings

**Reset synchronized settings**

To reset all settings to their defaults, sign in to Visual Studio, and then 
select Tools > Import and Export Settings to open the **Import** and **Export** 
Settings Wizard. Select Reset all settings and then follow the remaining steps 
of the wizard.
