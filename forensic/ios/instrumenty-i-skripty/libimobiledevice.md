---
description: >-
  либа и набор бинарей для взаимодействия с iphone нативно (не только с macos).
  Возможно крутая штука: TODO: разобраться
---

# libimobiledevice

## Install

site: [http://www.libimobiledevice.org/](http://www.libimobiledevice.org/)

### MacOS

`brew install libimobiledevice`

## ideviceimagemounter

example usage:  
1 mount Developer Disk Image  
`ideviceimagemounter [-u <udid>] <pathToDeveloperDiskImage> <pathToDeveloperDiskImageSignature>`  
-u - для случая с несколькими девайсами  
  
  
Где взять Developer Disk Image  
- На macos:   
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport/  
- [https://github.com/xushuduo/Xcode-iOS-Developer-Disk-Image.git](https://github.com/xushuduo/Xcode-iOS-Developer-Disk-Image.git)

## iproxy

Проброс портов over SSH USB

