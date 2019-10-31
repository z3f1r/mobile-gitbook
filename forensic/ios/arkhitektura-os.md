# Архитектура ОС

Safari: использует JS, just-in-time \(JIT\)   
Google Chrome: использует WebView

Secure Enclave Processor \(SEP\), который управляет ключами шифрования пользовательских данных

Мониторинг файловой системы Apple:  
[https://objective-see.com/blog/blog\_0x48.html](https://objective-see.com/blog/blog_0x48.html)

iOS boot chaining

{% file src="../../.gitbook/assets/iboot.tar" %}

![MVC &#x432; iOS &#x43F;&#x440;&#x438;&#x43B;&#x43E;&#x436;&#x435;&#x43D;&#x438;&#x44F;&#x445;](../../.gitbook/assets/mvc_v_ios_prilozheniyakh.png)

![&#x410;&#x440;&#x445;&#x438;&#x442;&#x435;&#x43A;&#x442;&#x443;&#x440;&#x430; iOS &#x43F;&#x440;&#x438;&#x43B;&#x43E;&#x436;&#x435;&#x43D;&#x438;&#x439;](../../.gitbook/assets/arkhitektura_ios_prilozhenii.png)

Стадии загрузки iOS \(Secure Boot\) - сверху вниз:  
- Boot ROM  
- Low Level Bootloader \(LLB\)  
- iBoot  
- iOS Kernel  
- IOS Apps



