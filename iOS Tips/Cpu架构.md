2、在iOS程序中移入静态库经常出现的问题：

Undefined symbols for architecture i386:静态库不能在使用i386架构的cpu设备上面运行

lipo - info 静态库：查看该静态库支持哪些架构

i386：iphone模拟器 3gs->iphone5

X86_64:iphone模拟器 5s->iphone6s

armv7:iphone真机 3gs->4s

armv7s:iphone真机    5->5c

arm64:iphone真机    5s->6plus