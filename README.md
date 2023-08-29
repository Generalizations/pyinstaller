# Pyinstaller+Inno Setup Packaging
Pyinstaller是一种将python程序打包成独立可执行的工具。Inno Setup是一种制作安装包的工具。<br>
- [安装准备](#Installation)
    - [Pyinstaller Installation](#Pyinstaller-Installation)
    - [Inno Setup Installation](#Inno-Setup-Installation)
- [Getting Started](#Getting-Started)
    - [Packaging](#Packaging)
        - [Pyinstaller Arguments](#Pyinstaller-Arguments)
            - [Position Arguments](#Position-Arguments)
            - [Optional Arguments](#Optional-Arguments)
            - [What To Generate](#What-To-Generate)
            - [What To Bundle, Where To Search](#What-To-Bundle-Where-To-Search)
            - [How To Generate](#How-To-Generate)
            - [Windows And Mac Os X Specific Options](#Windows-And-Mac-Os-X-Specific-Options)
            - [Windows Side-By-Side Assembly Searching Options (Advanced)](#Windows-Side--By--Side-Assembly-Searching-Options-(Advanced))
            - [Mac Os Specific Options](#Mac-Os-Specific-Options)
            - [Rarely Used Special Options](#Rarely-Used-Special-Options)
        - [添加版本资源](#Add-Version-Resource-To-executables)
## Installation
### Pyinstaller Installation
    pip install pyinstaller
    pyinstaller -v
### Inno Setup Installation
Inno Setup官网: https://jrsoftware.org/isinfo.php
## Getting Started
### Packaging
    pyinstaller /path/yourscript.py
生成dist文件、build文件、spec文件。dist文件夹包含相关依赖和可执行文件。build文件用来存放打包时的日志文件等。spec文件是可执行文件的相关配置文件。
> 打包完成后，build文件可直接删除，不影响可执行程序。<br/>
> spec文件可以打开后直接修改，也可以通过pyinstaller的参数进行修改，如pyi-makespec -w /path/yourscript.py/。修改完成后，将修改的配置打包到可执行文件pyinstaller /path/yourscript.spec。
### Pyinstaller Arguments
#### Position Arguments
|参数|作用|
|:---|:---|
|scriptname|Name of scriptfiles to be processed or exactly one .spec file. If a .spec file is specified, most options are unnecessary and are ignored.|
#### Optional Arguments
|参数|作用|说明|
|:---|:---|:---|
|-h, --help|显示所有帮助信息|无|
|-v, --version|显示pyinstaller版本号|无|
|--distpath DIR|打包生成的文件路径|默认:当前路径下的dist文件夹内|
|--workpath WORKPATH|打包过程中生成的日志文件路径|默认:当前目录下的build文件内|
|-y, --noconfirm|如果dist文件夹内已经存在生成文件，则不询问用户，直接覆盖|默认:询问用户是否覆盖|
|--upx-dir UPX_DIR|Path to UPX utility|默认: search the execution path|
|-a, --ascii|不包含unicode编码支持|默认:included if available|
|--clean|在本次编译之前，清空上次生成的各种文件|默认:不清除|
|--log-level LEVEL|编译时控制台信息中的详细信息|LEVEL may be one of TRACE, DEBUG, INFO, WARN, DEPRECATION, ERROR, FATAL (default: INFO). Also settable via and overrides the PYI_LOG_LEVEL environment variable.|
#### What To Generate
|参数|作用|说明|
|:---|:---|:---|
|-D, --onedir|生成包含一个可执行文件的one-folder(默认)|生成结果是一个目录，各种第三方依赖、资源和exe同时存储在该目录|
|-F, --onefile|生成只有可执行文件的one-file|生成结果是一个exe文件，所有的第三方依赖、资源和代码均被打包进该exe内，程序执行缓慢|
|--specpath DIR|指定spec文件的存储目录|默认:当前目录|
|-n NAME, --name NAME|生成的.exe文件和.spec的文件名|默认:和脚本同名|
|-n NAME, --name NAME|要分配给打包应用程序和spec文件的名称|默认:first script’s basename|
#### What To Bundle, Where To Search
|参数|作用|说明|
|:---|:---|:---|
|--add-data <SRC;DEST or SRC:DEST>|打包其他非二进制资源,例如:图片|用法：pyinstaller /path/yourscript.py --add-data=src;dest。windows以;分割，linux以:分割|
|--add-binary <SRC;DEST or SRC:DEST>|打包额外的代码|用法:同–add-data。与–add-data不同的是，用binary添加的文件，pyi会分析它引用的文件并把它们一同添加进来|
|-p DIR, --paths DIR|A path to search for imports (like using PYTHONPATH).|允许多个路径，中间用':'隔开，等价于向spec文件提供pathex参数|
|--hidden-import MODULENAME, --hiddenimport MODULENAME|Name an import not visible in the code of the script(s). |用于打包时有些引入的MODULENAME没有找到，可多次使用|
|--collect-submodules MODULENAME|收集指定的包或模块内所有的子模块|可多次使用|
|--collect-data MODULENAME, --collect-datas MODULENAME|收集指定的包或模块中的所有数据文件|可多次使用|
|--collect-binaries MODULENAME|收集指定的包或模块内所有的二进制文件|可多次使用|
|--collect-all MODULENAME|收集指定的包或模块中所有的子模块、数据文件和二进制文件|可多次使用|
|--copy-metadata PACKAGENAME|复制指定包的元数据|可多次使用|
|--recursive-copy-metadata PACKAGENAME|复制指定包及其所有依赖项的元数据|可多次使用|
|--additional-hooks-dir HOOKSPATH|An additional path to search for hooks.|可多次使用|
|--runtime-hook RUNTIME_HOOKS|Path to a custom runtime hook file. |如果设置了此参数，则runtime-hook会在运行main.py之前被运行|
|--exclude-module EXCLUDES|Optional module or package (the Python name, not the path name) that will be ignored (as though it was not found). |打包时忽略用不到的依赖库，减少文件大小|
|--splash IMAGE_FILE|(EXPERIMENTAL) Add an splash screen with the image IMAGE_FILE to the application.|The splash screen can display progress updates while unpacking.|
#### How To Generate
|参数|作用|说明|
|:---|:---|:---|
|-d {all,imports,bootloader,noarchive},<br> --debug {all,imports,bootloader,noarchive}|应用程序执行时，输出log，有助于排查错误|默认:不输出log|
|--python-option PYTHON_OPTION|运行时指定一个命令行选项传给python解释器|Currently supports “v” (equivalent to “–debug imports”), “u”, and “W <warning control>”.|
|-s, --strip|Apply a symbol-table strip to the executable and shared libs|不建议在Windows系统使用|
|--noupx|即使UPX可用，也不要使用|works differently between Windows and *nix|
|--upx-exclude FILE|使用upx时，防止二进制文件被压缩。|用于UPX压缩时，损坏了某些二进制文件。FILE是没有路径的二进制文件的文件名，该参数可多次使用|
#### Windows And Mac Os X Specific Options
|参数|作用|说明|
|:---|:---|:---|
|-c, --console, --nowindowed|可执行文件工作时显示控制台窗口(默认)|On Windows this option has no effect if the first script is a ‘.pyw’ file.|
|-w, --windowed, --noconsole|Windows and Mac OS X:可执行文件工作时不显示控制台窗口|On Mac OS this also triggers building a Mac OS .app bundle.<br> On Windows this option is automatically set if the first script is a ‘.pyw’ file.<br> This option is ignored on *NIX systems.|
|-i <FILE.ico or FILE.exe,ID or FILE.icns or Image or "NONE">,<br> --icon <FILE.ico or FILE.exe,ID or FILE.icns or Image or "NONE">|给可执行文件添加图片|FILE.ico: apply the icon to a Windows executable.<br> FILE.exe,ID: extract the icon with ID from an exe.<br> FILE.icns: apply the icon to the .app bundle on Mac OS. <br>如果输入的图像文件不是平台格式（Windows上为ico，Mac上为icns），PyInstaller会尝试使用Pillow将图标转换为正确的格式。|
|--disable-windowed-traceback|Disable traceback dump of unhandled exception in windowed (noconsole) mode (Windows and macOS only), and instead display a message that this feature is disabled.|无|
> 如果需要安装Pillow，pip install Pillow
#### Windows Specific Options
|参数|作用|说明|
|:---|:---|:---|
|--version-file FILE|添加版本信息文件|用法:pyinstaller --version-file version_file_info.txt|
|--no-embed-manifest|生成一个额外的.exe.manifest文件|仅适用于onedir模式，在onefile模式中，有没有设置这个参数，manifest都是内嵌在.exe中的|
|-r RESOURCE,<br> --resource RESOURCE|向Windows可执行文件添加或更新资源。|The RESOURCE is one to four items, FILE[,TYPE[,NAME[,LANGUAGE]]].<br>FILE可以是一个数据文件或exe/dll文件<br>对于数据文件，至少TYPE和NAME必须被指定，LANGUAGE默认0或也许被指定为wildcard *，更新给定的TYPE和NAME的所有资源<br>对于exe/dll文件，如果TYPE, NAME 和 LANGUAGE被忽略或者被指定为wildcard *，所有资源文件将被添加/更新到最终的可执行文件|
|--uac-admin|创建一个Manifest，该Manifest将在应用程序启动时请求提升。|无|
|--uac-uiaccess|允许升级应用程序与远程桌面一起工作。|无|
#### Windows Side-By-Side Assembly Searching Options (Advanced)
|参数|作用|说明|
|:---|:---|:---|
|--win-private-assemblies|Any Shared Assemblies bundled into the application will be changed into Private Assemblies.|This means the exact versions of these assemblies will always be used, and any newer versions installed on user machines at the system level will be ignored.|
|--win-no-prefer-redirects|While searching for Shared or Private Assemblies to bundle into the application, PyInstaller will prefer not to follow policies that redirect to newer versions, and will try to bundle the exact versions of the assembly.|无|
#### Mac Os Specific Options
|参数|作用|说明|
|:---|:---|:---|
|--argv-emulation|Enable argv emulation for macOS app bundles. |If enabled, the initial open document/URL event is processed by the bootloader and the passed file paths or URLs are appended to sys.argv.|
|--osx-bundle-identifier BUNDLE_IDENTIFIER|Mac OS .app bundle identifier is used as the default unique program name for code signing purposes.|The usual form is a hierarchical name in reverse DNS notation.<br>For example: com.mycompany.department.appname (default: first script’s basename)|
|--target-architecture ARCH, --target-arch ARCH|Target architecture (macOS only; valid values: x86_64, arm64, universal2).|Enables switching between universal2 and single-arch version of frozen application (provided python installation supports the target architecture).<br> If not target architecture is not specified, the current running architecture is targeted.|
|--codesign-identity IDENTITY|Code signing identity (macOS only). |Use the provided identity to sign collected binaries and generated executable. <br>If signing identity is not provided, ad- hoc signing is performed instead.|
|--osx-entitlements-file FILENAME|Entitlements file to use when code-signing the collected binaries (macOS only).|无|
#### Rarely Used Special Options
|参数|作用|说明|
|:---|:---|:---|
|--runtime-tmpdir PATH|Where to extract libraries and support files in onefile-mode.|If this option is given, the bootloader will ignore any temp-folder location defined by the run-time OS.<br> The _MEIxxxxxx-folder will be created here. Please use this option only if you know what you are doing.|
|--bootloader-ignore-signals|Tell the bootloader to ignore signals rather than forwarding them to the child process. |Useful in situations where for example a supervisor process signals both the bootloader and the child (e.g., via a process group) to avoid signalling the child twice.|
### Add Version Resource To executables
+ Capturing Windows Version Data
```
pyi-grab_version executable_with_version_resource
```
+ Editing The Version Information To Adapt It To Your Program<br>
filevers(1,2,3,4)<br>
prodvers(1,2,3,4)<br>
filevers(文件版本)和prodvers(产品版本)需要4个元素，分别是主版本号、次版本号、修订版本号、编译版本号,根据实际情况修改
```
StringTable——包含启动程序要显示的版本信息
StringFileInfo(
[
 StringTable(
    '',   语言信息 如：080404B0表示中文
    [StringStruct('CompanyName', ''),  开发产品公司名称
    StringStruct('FileDescription', ''), 文件说明
    StringStruct('FileVersion', ''), 文件版本
    StringStruct('InternalName', ''), 内部名称
    StringStruct('LegalCopyright', ''), 版权
    StringStruct('OriginalFilename', ''), 源文件名
    StringStruct('ProductName', ''),  产品名称
    StringStruct('Comments', ''),  备注
    StringStruct('LegalTrademarks', ''), 文件的所有注册商标信息
    StringStruct('ProductVersion', '1.0.0.0')])  产品版本
]),
VarFileInfo([VarStruct('Translation', [0x0804, 1200])])  语言信息 0x0804：中文简体,1200：中国
```
+ Add version_file to executables
    + 方式一 
    ```
    pyi-set_version version_text_file executable_file
    ```
    > pyi-set_version：读取由pyi-grab_version编写的版本文本文件，将其转换为版本资源，并将该资源打包到指定的executable_file中。
    + 方式二
    ```
    pyi-makespec --version-file=version_text_file /path/yourscript.py 
    pyinstaller /path/yourscript.spec
    ```
    > pyi-makespec --version-file 将版本文本文件写入配置文件<br>
    > pyinstaller /path/yourscript.spec 将版本文本文件转为版本资源打包到可执行文件
## 
