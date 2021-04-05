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

## Usage

### ideviceimagemounter

example usage:  
1 mount Developer Disk Image  
`ideviceimagemounter [-u <udid>] <pathToDeveloperDiskImage> <pathToDeveloperDiskImageSignature>`  
-u - для случая с несколькими девайсами  
  
  
Где взять Developer Disk Image  
- На macos:   
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport/  
- [https://github.com/xushuduo/Xcode-iOS-Developer-Disk-Image.git](https://github.com/xushuduo/Xcode-iOS-Developer-Disk-Image.git)

### iproxy

Проброс портов over SSH USB

## Аналог на Go?

Go-iOS was inspired by the wonderful libimobiledevice. It can do all of what libimobiledevice can do and more. Highlights:

* run XCTests including WebdriverAgent on Linux, Windows and Mac
* start and stop apps
* Use a debug proxy to reverse engineer every tool Mac OSX has, so you can contrib to go-ios or build your own
* use Accessibility Inspector APIs

[https://github.com/danielpaulus/go-ios](https://github.com/danielpaulus/go-ios)

