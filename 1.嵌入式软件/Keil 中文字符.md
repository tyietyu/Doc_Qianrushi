直接在keil工程代码中加入中文字符，编译会报错。

解决方法：

在target选项卡中的“C/C++”选项卡下的“Misc Controls ”选项中添加 '--no-multibyte-chars' 即可解决。

![image-20250422143530221](./image/Keil 中文字符.assets/image-20250422143530221.png)