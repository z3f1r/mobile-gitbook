# Bluetooth

Класс девайса по 3 байтам, их кодирующим: 

{% file src="../.gitbook/assets/cod\_definition.pdf" %}

Где искать информацию на девайсе:  
**Android**  
Скорее всего из:   
/data/misc/bluedroid/bt\_config.xml   
Но можно ещё проверить из:   
/data/misc/bluetoothd\*   
/data/misc/bluedroid/bt\_config.conf  
  
**iOS**  
/private/var/mobile/Library/Preferences/com.apple.MobileBluetooth.devices.plist  
/private/var/mobile/Applications/[systemgroup.com](http://systemgroup.com).apple.bluetooth/Library/Preferences/com.apple.MobileBluetooth.devices.plist  
/private/var/containers/Shared/SystemGroup/&lt;GUID&gt;/Library/Preferences/com.apple.MobileBluetooth.devices.plist



