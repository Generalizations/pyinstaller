一、安装与打包相关的包
    1.pyinstaller——python打包工具
      pip install pyinstaller
    2.pillow———图像处理库（给启动程序添加图标时，对图片格式进行处理）
      pip install pillow
      
二、程序打包
    1.pyinstaller常用参数
      pyinstaller -D 将所有依赖文件打包到一个文件夹中，包含多个文件(包含启动程序的依赖文件)
      pyinstaller -F 仅生成一个文件.exe 
      pyinstaller -w 程序运行时不显示控制台
      pyinstaller -c 程序运行时显示控制台
      pyinstaller --add-data 可打包除了.py的文件 pyinstaller --add-data=源地址;目标地址
      pyinstaller -i 指定启动程序的图标
      pyinstaller --version-file 添加版本信息
      pyinstaller -p 打包时搜索模块路径
      pyinstaller -h 查看pyinstaller的所有参数
      
    2.pyi-makespec 只生成app.spec配置文件，可以添加参数编辑spec文件，可添加的参数与pyinstaller的参数相同
      -i 指定启动程序的图标
      --version-file=file_version_info.txt  配置文件app.spec添加版本信息
      
    3.pyinstaller app.spec
      将启动程序按照spec配置
      
    4.指定启动程序的版本信息编辑  
      选择一个具有标准版本信息的启动程序，放于dist文件夹下和app.exe同级目录下：
      pyi-grab_version 带有标准版本信息的exe
      生成file_version_info.txt文件，获取标准的版本信息格式，打开文件，编辑以下启动程序的相关信息
      filevers(文件版本)和prodvers(产品版本)需要4个元素，分别是主版本号、次版本号、修订版本号、编译版本号
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
    VarFileInfo([VarStruct('Translation', [0x0804, 1200])])  语言信息 0x0804：中文简体，1200：中国
  ]
    
  
    
    
      
      
    
