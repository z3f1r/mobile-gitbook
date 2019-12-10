# Frida IPA dump

Новая .ipa декрипт \(jb - checkra1n\) - непонятно работает ли вообще: [https://github.com/ChiChou/bagbak](https://github.com/ChiChou/bagbak)

Другой вариант \(рабочий\): [https://github.com/AloneMonkey/frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)  
Использование:  
- brew install usbmuxd - пакет для ssh over usb  
- iproxy 2222 22 -&gt; перекидываем порты \(iproxy - часть usbmuxd\)  
- подсоединяем iphone через usb-провод  
- python3 dump.py -l - список пакетов  
- python3 dump.py &lt;packet-id&gt;  






