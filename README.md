> [!IMPORTANT]
> Crucial information necessary for users to succeed.
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
            - [Windows Side-By-Side Assembly Searching Options](#Windows-Side-By-Side-Assembly-Searching-Options)
            - [Mac Os Specific Options](#Mac-Os-Specific-Options)
            - [Rarely Used Special Options](#Rarely-Used-Special-Options)
        - [添加版本资源](#Add-Version-Resource-To-executables)
    - [安装包制作](#安装包制作)
## Installation
### Pyinstaller Installation
    pip install pyinstaller
    pyinstaller -v
执行`pyinstaller -v`，若结果显示pyinstaller版本号，则安装成功，如下图所示：<br>
![19](https://github.com/wangrui11111/pyinstaller/assets/142973887/adbf2498-a3cd-4d06-b6c2-b3d603022198)
### Inno Setup Installation
Inno Setup官网: https://jrsoftware.org/isinfo.php <br>
![1](https://github.com/wangrui11111/pyinstaller/assets/142973887/95ea14e7-ac6b-4c91-930f-55129a2f9539)<br>

![2](https://github.com/wangrui11111/pyinstaller/assets/142973887/774ada6e-8341-4ff8-8aae-32137e83b9b7)
## Getting Started
### Packaging
    pyinstaller /path/yourscript.py
- 生成**dist**文件、**build**文件、.**spec**文件。
- **dist**文件夹：包含**相关依赖**和**可执行文件**。
- **build**文件夹：用来存放打包时的**临时**文件等。 打包完成后，build文件可直接**删除**，不影响可执行程序。
- .**spec**文件：是可执行文件的相关**配置**文件。此文件可以打开后直接修改，也可以通过**pyinstaller**的**参数**进行修改，如`pyi-makespec -w /path/yourscript.py`。修改完成后，将修改的配置打包到可执行文件`pyinstaller /path/yourscript.spec`。
#### Pyinstaller Arguments
##### Position Arguments
|参数|作用|
|:---|:---|
|`scriptname`|<details><summary>**Name of scriptfiles** to be processed or exactly **one .spec file**.</summary> <p>If a .spec file is specified, most options are unnecessary and are ignored.</p></details>|
##### Optional Arguments
|参数|作用|
|:---|:---|
|`-h, --help`|显示所有**pyinstaller**的**帮助**信息|
|`-v, --version`|显示**pyinstaller版本号**|
|`--distpath DIR`|<details><summary>打包生成的文件路径</summary><p>默认：当前路径下的**dist**文件夹内</p></details>|
|`--workpath WORKPATH`|<details><summary>打包过程中生成的临时文件路径</summary><p>默认：当前目录下的**build**文件内</p></details>|
|`-y, --noconfirm`|<details><summary>如果dist文件夹内已经存在生成文件，则**不询问**用户，直接覆盖</summary><p>默认：**询问**用户是否覆盖</P></details>|
|`--upx-dir UPX_DIR`|<details><summary>Path to UPX utility</summary><p>默认：search the **execution** path</p></details>|
|`-a, --ascii`|<details><summary>**不包含unicode**编码支持</summary><P>默认：**included** if available</p></details>|
|`--clean`|<details><summary>在本次编译之前，**清空上次**生成的各种文件</summary><p>默认：**不清除**</P></details>|
|`--log-level LEVEL`|<details><summary>编译时控制台信息中的详细信息</summary><p>LEVEL may be one of **TRACE**, **DEBUG**, **INFO**, **WARN**, **DEPRECATION**, **ERROR**, **FATAL** (default: INFO).<br> Also **settable via** and overrides the **PYI_LOG_LEVEL** environment variable.</p></details>|
##### What To Generate
|参数|作用|
|:---|:---|
|`-D, --onedir`|<details><summary>生成包含一个可执行文件的**one-folder**(default)</summary><p>生成结果是一个**目录**，各种第三方依赖、资源和.exe**同时**存储在该目录</p></details>|
|`-F, --onefile`|<details><summary>生成只有可执行文件的**one-file**</summary><p>生成结果是一个.**exe**文件，所有的第三方依赖、资源和代码均被打包进该.exe内，程序执行**缓慢**</p></details>|
|`--specpath DIR`|<details><summary>指定.**spec**文件的存储目录</summary><p>默认：当前目录</p></details>|
|`-n NAME, --name NAME`|<details><summary>要分配给打包生成的.**exe**和.**spec**文件的名称</summary><p>默认：**first script’s basename**</p></details>|
##### What To Bundle, Where To Search
|参数|作用|
|:---|:---|
|`--add-data <SRC;DEST or SRC:DEST>`|<details><summary>打包**非二进制**资源,例如:图片</summary><p>用法：`pyinstaller /path/yourscript.py --add-data=src;dest`。**windows以;分割，linux以:分割**</p></details>|
|`--add-binary <SRC;DEST or SRC:DEST>`|<details><summary>打包**非二进制**资源</summary><p>用法:**同**`–add-data`。与`–add-data`不同的是，用binary添加的文件，pyi会分析它引用的文件并把它们**一同添加**进来</p></details>|
|`-p DIR, --paths DIR`|<details><summary>A path to search for **imports (like using PYTHONPATH)**.</summary><p>允许多个路径，中间用'**:**'隔开，等价于向.**spec**文件提供**pathex**参数</p></details>|
|`--hidden-import MODULENAME`,<br> `--hiddenimport MODULENAME`|<details><summary>Name an import **not visible** in the code of the script(s).</summary><p>用于打包时有些引入的**MODULENAME没有找到**，可多次使用</P></details>|
|`--collect-submodules MODULENAME`|<details><summary>收集指定的包或模块内所有的**子模块**</summary><p>可多次使用</p></details>|
|`--collect-data MODULENAME`,<br>` --collect-datas MODULENAME`|<details><summary>收集指定的包或模块中的所有**数据文件**</summary><p>可多次使用</p></details>|
|`--collect-binaries MODULENAME`|<details><summary>收集指定的包或模块内所有的**二进制文件**</summary><p>可多次使用</p></details>|
|`--collect-all MODULENAME`|<details><summary>收集指定的包或模块中所有的**子模块、数据文件和二进制文件**</summary><p>可多次使用</p></details>|
|`--copy-metadata PACKAGENAME`|<details><summary>复制指定包的**元数据**</summary><p>可多次使用</p></details>|
|`--recursive-copy-metadata PACKAGENAME`|<details><summary>复制指定包及其所有**依赖项的元数据**</summary><p>可多次使用</p></details>|
|`--additional-hooks-dir HOOKSPATH`|<details><summary>An additional path to search for **hooks**.</summary><p>可多次使用</p></details>|
|`--runtime-hook RUNTIME_HOOKS`|<details><summary>Path to a **custom runtime** hook file.</summary><p>如果设置了此参数，则runtime-hook会在运行main.py**之前**被运行</p></details>|
|`--exclude-module EXCLUDES`|<details><summary>Optional module or package (the Python name, not the path name)<br> that will be **ignored** (as though it was not found).</summary><p>打包时忽略用不到的依赖库，减少文件大小</p></details>|
|`--splash IMAGE_FILE`|<details><summary>(EXPERIMENTAL) Add an splash screen with the image IMAGE_FILE to the application.</summary><p>The splash screen can display progress updates while unpacking.</p></details>|
##### How To Generate
|参数|作用|
|:---|:---|
|`-d {all,imports,bootloader,noarchive}`,<br> `--debug {all,imports,bootloader,noarchive}`|<details><summary>应用程序执行时，输出log，有助于排查错误</summary><p>默认:不输出log</p></details>|
|`--python-option PYTHON_OPTION`|<details><summary>运行时指定一个命令行选项传给python解释器</summary><p>Currently supports “v” (equivalent to “–debug imports”), “u”, and “W <warning control>”.</p></details>|
|`-s, --strip`|<details><summary>Apply a symbol-table strip to the executable and shared libs</summary><p>不建议在Windows系统使用</p></details>|
|`--noupx`|<details><summary>禁止使用UPX</summary><p>works differently between Windows and *nix</p></details>|
|`--upx-exclude FILE`|<details><summary>使用upx时，**防止二进制文件被压缩**。</summary><p>用于UPX压缩时，损坏了某些二进制文件。**FILE**是没有路径的**二进制文件**的文件名，该参数可多次使用</p></details>|
##### Windows And Mac Os X Specific Options
|参数|作用|
|:---|:---|
|`-c, --console, --nowindowed`|<details><summary>可执行文件工作时**显示**控制台窗口(默认)</summary><p>On Windows this option has no effect if the first script is a ‘.pyw’ file.</p></details>|
|`-w, --windowed, --noconsole`|<details><summary>Windows and Mac OS X:可执行文件工作时**不显示**控制台窗口</summary><p>On Mac OS this also triggers building a Mac OS .app bundle.<br> On Windows this option is automatically set if the first script is a ‘.pyw’ file.<br> This option is ignored on *NIX systems.</p></details>|
|`-i <FILE.ico or FILE.exe,ID or FILE.icns or Image or "NONE">`,<br> `--icon <FILE.ico or FILE.exe,ID or FILE.icns or Image or "NONE">`|<details><summary>给可执行文件**添加图片**</summary><p>FILE.ico: apply the icon to a Windows executable.<br> FILE.exe,ID: extract the icon with ID from an exe.<br> FILE.icns: apply the icon to the .app bundle on Mac OS. <br>如果输入的图像文件不是平台格式（Windows上为ico，Mac上为icns），PyInstaller会尝试使用[Pillow](#安装Pillow)将图标转换为正确的格式。</p></details>|
|`--disable-windowed-traceback`|Disable traceback dump of unhandled exception in windowed (noconsole) mode (Windows and macOS only), and instead display a message that this feature is disabled.|
> - 安装Pillow(图像处理)，`pip install Pillow`，安装完成后，输入`pip show Pillow`，若显示版本信息，则安装成功，如下图：<br>
> ![20](https://github.com/wangrui11111/pyinstaller/assets/142973887/afb25126-d257-4294-9396-5ea8c689c8d5)
##### Windows Specific Options
|参数|作用|
|:---|:---|
|`--version-file FILE`|<details><summary>添加**版本信息**文件</summary><p>用法:`pyinstaller --version-file version_file_info.txt`</p></details>|
|`--no-embed-manifest`|<details><summary>生成一个**额外**的.**exe**.**manifest**文件</summary><p>**仅适用于onedir**模式，在onefile模式中，有没有设置这个参数，manifest都是内嵌在.exe中的|
|`-r RESOURCE, --resource RESOURCE`|<details><summary>向**Windows**可执行文件**添加或更新**资源。</summary><p>The RESOURCE is **one to four items**, **FILE[,TYPE[,NAME[,LANGUAGE]]]**.<br>**FILE**可以是一个**数据文件**或.**exe/dll文件**<br>对于数据文件，**至少TYPE**和**NAME**必须被指定，LANGUAGE默认0或也许被指定为wildcard *，更新给定的TYPE和NAME的所有资源<br>对于exe/dll文件，如果TYPE, NAME 和 LANGUAGE被忽略或者被指定为wildcard *，所有资源文件将被添加/更新到最终的可执行文件</p></details>|
|`--uac-admin`|创建一个Manifest，该Manifest将在应用程序启动时请求提升。
|`--uac-uiaccess`|允许升级应用程序与远程桌面一起工作。
##### Windows Side-By-Side Assembly Searching Options
|参数|作用|
|:---|:---|
|`--win-private-assemblies`|<details><summary>Any Shared Assemblies bundled into the application will be **changed into Private** Assemblies.</summary><p>This means the exact versions of these assemblies will always be used, and any newer versions installed on user machines at the system level will be ignored.</p></details>|
|`--win-no-prefer-redirects`|While searching for Shared or Private Assemblies to bundle into the application, PyInstaller will <br>prefer not to follow policies that redirect to newer versions, and will try to **bundle the exact <br>versions** of the assembly.|
##### Mac Os Specific Options
|参数|作用|
|:---|:---|
|`--argv-emulation`|<details><summary>Enable argv emulation for macOS app bundles.</summary><p>If enabled, the initial open document/URL event is processed by the bootloader and the passed file paths or URLs are appended to sys.argv.</p></details>|
|`--osx-bundle-identifier BUNDLE_IDENTIFIER`|<details><summary>Mac OS .app bundle identifier is used as the default **unique** program name for code signing purposes.</summary><p>The usual form is a hierarchical name in reverse DNS notation.<br>For example: com.mycompany.department.appname (default: first script’s basename)</p></details>|
|`--target-architecture ARCH, --target-arch ARCH`|<details><summary>Target architecture (macOS only; valid values: x86_64, arm64, universal2).</summary><p>Enables switching between universal2 and single-arch version of frozen application (provided python installation supports the target architecture).<br> If not target architecture is not specified, the current running architecture is targeted.</p></details>|
|`--codesign-identity IDENTITY`|<details><summary>Code signing identity (macOS only).</summary><p>Use the provided identity to sign collected binaries and generated executable. <br>If signing identity is not provided, ad- hoc signing is performed instead.</p></details>|
|`--osx-entitlements-file FILENAME`|Entitlements file to use when code-signing the collected binaries (macOS only).
##### Rarely Used Special Options
|参数|作用|
|:---|:---|
|`--runtime-tmpdir PATH`|<details><summary>Where to extract libraries and support files in **onefile-mode**.</summary><p>If this option is given, the bootloader will ignore any temp-folder location defined by the run-time OS.<br> The _MEIxxxxxx-folder will be created here. Please use this option only if you know what you are doing.</p></details>|
|`--bootloader-ignore-signals`|<details><summary>Tell the bootloader to ignore signals rather than forwarding them to the child process.</summary><p>Useful in situations where for example a supervisor process signals both the bootloader and the child (e.g., via a process group) to avoid signalling the child twice.</p></details>|
#### Add Version Resource To executables
+ Capturing Windows Version Data
```
pyi-grab_version executable_with_version_resource.exe
```
在当前目录生成一个版本文本文件，内容为executable_with_version_resource.exe的版本信息
+ Editing The Version Information To Adapt It To Your Program<br>
filevers(1,2,3,4)<br>
prodvers(1,2,3,4)<br>
filevers(文件版本)和prodvers(产品版本)需要**4**个元素，分别是**主版本号**、**次版本号**、**修订版本号**、**编译版本号**,根据实际情况修改
````
```
kids=[
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
        StringStruct('ProductVersion', '')])  产品版本
    ]),
    VarFileInfo([VarStruct('Translation', [0x0804, 1200])])  语言信息 0x0804：中文简体,1200：中国 可根据需求修改
   ]
```
````


+ Add version_file to executables
    + 方式一 
    ```
    pyi-set_version version_text_file executable_file
    ```
    > `pyi-set_version`：读取由`pyi-grab_version`编写的版本文本文件，将其转换为版本资源，并将该资源打包到指定的executable_file中。
    + 方式二
    ```
    pyi-makespec --version-file=version_text_file /path/yourscript.py 
    pyinstaller /path/yourscript.spec
    ```
    > `pyi-makespec --version-file` 将版本文本文件写入配置文件<br>
    > `pyinstaller /path/yourscript.spec` 将版本文本文件转为版本资源打包到可执行文件
### 安装包制作
- 打开Inno Setup后，选择创建一个脚本，并使用导向制作，如下图所示：<br>
![3](https://github.com/wangrui11111/pyinstaller/assets/142973887/a803512d-fd03-4be8-9e57-5c653bcc2151)


- 下一步<br>
![4](https://github.com/wangrui11111/pyinstaller/assets/142973887/461aa1f4-9b1a-41ff-8bc4-595548c4867a)


- 根据实际情况，填写以下相关信息，**应用程序名称（必填）**、**应用程序版本（必填）**、应用程序发布者、应用程序网站，如下图所示：<br>
![5](https://github.com/wangrui11111/pyinstaller/assets/142973887/b632bdd2-9788-4b46-8b51-4fbfa6f05b29)


- 配置应用程序安装路径相关信息，如下图所示：<br>
![6](https://github.com/wangrui11111/pyinstaller/assets/142973887/60593934-2ab1-4310-92be-ae4ba35a3d93)


- 添加应用程序及其运行所需要的依赖文件：<br>
![7](https://github.com/wangrui11111/pyinstaller/assets/142973887/6ef0968a-9235-4ffd-89a2-1569206787f6)


- 填写关联到应用程序的文件类型名，如下图所示：<br>
![8](https://github.com/wangrui11111/pyinstaller/assets/142973887/d8f1b7d2-9b13-4805-bc41-6119f260fa5f)


- 创建快捷方式：<br>
![9](https://github.com/wangrui11111/pyinstaller/assets/142973887/1445d1f9-9f78-43cd-9ab3-6a4f08b12eda)


- 添加安装过程中显示的信息文件：<br>
![10](https://github.com/wangrui11111/pyinstaller/assets/142973887/29eb70fb-84e9-46c9-81b2-f984fc2cded7)


- 配置安装模式：<br>
![11](https://github.com/wangrui11111/pyinstaller/assets/142973887/d01753ca-f1a5-478a-a3bd-b88d38453eda)


- 配置安装语言：<br>
![12](https://github.com/wangrui11111/pyinstaller/assets/142973887/67a1b1ec-1c26-4a16-b089-5e83185b10fd)


- 配置安装包信息：<br>
![13](https://github.com/wangrui11111/pyinstaller/assets/142973887/48af923a-1986-4e71-81c5-84c59a3b11bc)


- 下一步：<br>
![14](https://github.com/wangrui11111/pyinstaller/assets/142973887/096df849-a0bd-4b64-8695-5b0096f80295)


- Finish：<br>
![15](https://github.com/wangrui11111/pyinstaller/assets/142973887/a26b13a8-7ca9-4760-8bc1-ed240c02208d)<br>
![16](https://github.com/wangrui11111/pyinstaller/assets/142973887/206499a0-1a54-4bdc-a9b1-7233264b0ac9)<br>
![17](https://github.com/wangrui11111/pyinstaller/assets/142973887/a0a9a334-fc0d-4c4d-9520-d2749835ad43)<br>
![18](https://github.com/wangrui11111/pyinstaller/assets/142973887/2d31f741-f924-4dd7-99bb-55b0580362df)
