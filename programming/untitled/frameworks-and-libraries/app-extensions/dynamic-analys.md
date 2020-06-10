# Dynamic Analys

Для динамического анализа делаем следующее:

* Inspecting the items being shared
* Identifying the app extensions involved

## Inspecting the items being shared

Делаем хук на `NSExtensionContext - inputItems`

Далее, например, в приложении Notes нажимаем кнопку Share \(или совершаем любое действие, которое задействует наше расширение\), и выбираем целью исследуемое приложение. Это приведет к вызову хука. 

Пример:

```text
(0x1c06bb420) NSExtensionContext - inputItems
0x18284355c Foundation!-[NSExtension _itemProviderForPayload:extensionContext:]
0x1828447a4 Foundation!-[NSExtension _loadItemForPayload:contextIdentifier:completionHandler:]
0x182973224 Foundation!__NSXPCCONNECTION_IS_CALLING_OUT_TO_EXPORTED_OBJECT_S3__
0x182971968 Foundation!-[NSXPCConnection _decodeAndInvokeMessageWithEvent:flags:]
0x182748830 Foundation!message_handler
0x181ac27d0 libxpc.dylib!_xpc_connection_call_event_handler
0x181ac0168 libxpc.dylib!_xpc_connection_mach_event
...
RET: (
"<NSExtensionItem: 0x1c420a540> - userInfo:
{
    NSExtensionItemAttachmentsKey =     (
    "<NSItemProvider: 0x1c46b30e0> {types = (\n \"public.plain-text\",\n \"public.file-url\"\n)}"
    );
}"
)
```

Видим, что:

* Используется низкоуровневая системная библиотека \(фреймворк\) libxpc.dylib. XPC - межпроцессное взаимодействие. Конкретнее, класс - NSXPCConnection
* UTIs, определенные в NSItemProvider - public.plain-text и public.file-url \(которые включены в NSExtensionActivationRule в Info.plist\) 

## Identifying the app extensions involved

Узнать, какое именно расширение обрабатывает ваш запрос - хук на метод `NSExtension - _plugIn`

 Пример:

```text
(0x1c0370200) NSExtension - _plugIn
RET: <PKPlugin: 0x1163637f0 ph.telegra.Telegraph.Share(5.3) 5B6DE177-F09B-47DA-90CD-34D73121C785
1(2) /private/var/containers/Bundle/Application/15E6A58F-1CA7-44A4-A9E0-6CA85B65FA35
/Telegram X.app/PlugIns/Share.appex>

(0x1c0372300)  -[NSExtension _plugIn]
RET: <PKPlugin: 0x10bff7910 com.apple.mobilenotes.SharingExtension(1.5) 73E4F137-5184-4459-A70A-83
F90A1414DC 1(2) /private/var/containers/Bundle/Application/5E267B56-F104-41D0-835B-F1DAB9AE076D
/MobileNotes.app/PlugIns/com.apple.mobilenotes.SharingExtension.appex>
```

как видим, в примере отрабатывают 2 расширения

Для более глубокого анализа, надо исследовать библиотеку libxpc.dylib вместе с frida-trace. Там есть много интересных методов.

