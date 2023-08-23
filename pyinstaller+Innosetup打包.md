一、安装与打包相关的包
    1.pyinstaller——python打包工具
      pip install pyinstaller
    2.pillow———图像处理库（给启动程序添加图标时，对图片进行处理）
      pip install pillow
二、程序打包
    1.pyinstaller –D ux_chat/app.py
      生成dist文件夹、build文件夹、app.sepc文件
    2.pyi-makespec -D -w -i ux_chat/images/common/ux.ico --version-file=file_version_info.txt ux_chat/app.py
      修改app.spec配置文件
      -w 去掉执行程序时的控制台
      -i ux_chat/images/common/ux.ico 给app.sepc文件添加启动程序的图标
      --version-file=file_version_info.txt 给app.spec文件添加版本信息配置
    3.选择一个带有标准版本信息的exe,添加到dist文件夹下中app.exe的项目目录下
      pyi-grab_version dist/app/YoudaoDict.exe
      生成file_version_info.txt文件，获取标准的版本信息格式，打开文件，添加打包程序所需的版本信息
    4.pyinstaller app.spec
      版本信息生成，启动程序图标添加成功
三、制作安装包
    1.安装InnoSetup,安装路径：https://jrsoftware.org/isinfo.php
    2.
    
  
    
    
      
      
    
