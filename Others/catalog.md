### 1：nexus/pixel 刷机攻略

1 梯子下载相应版本手机型号的 
  
  android 系统镜像 https://developers.google.com/android/images#bullhead

2 终端 确保 adb 环境变量

  adb reboot bootloader

  fastboot oem unlock （解锁设备）
 
3 解压 zip 
  .
  ├── bootloader-bullhead-bhz32c.img
  ├── flash-all.bat
  ├── flash-all.sh
  ├── flash-base.sh
  ├── image-bullhead-opm7.181205.001.zip
  └── radio-bullhead-m8994f-2.6.42.5.03.img

  当 手机处于 boot 模式下

  执行 ./flash-all.sh (mac/linux/unix)
  
  等待系统安装重启

