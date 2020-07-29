# objection

## Info

Тулза над фридой. Умеет куча всего.

## Написание плагинов

Структура: 

```text
name_plugin/         - <name_plugin> - название плагина
    __init__.py  - код
```

Запуск:

```text
objection --gadget "ru.sberbank.*" explore -P "/Users/**/Work/pentest/projects/**/scripts" -s "plugin bypass info"
```

-P - указать папку с плагинами  
-s - запустить плагин во время запуска приложения

Пример плагина \(файл \_\_init\_\_.py\)

```python
__description__ = "***: JB Bypass (***)"

from objection.utils.plugin import Plugin

s = """
rpc.exports = {
    test: function() {
        console.log("[+] Jailbreak Detection Bypass"); 
        if (ObjC.available) {
            try {  
                var module = "***";  // finded by frida-trace -U -f ru.sberbank.*** -i "sbf_***"
                
                var functionName = "sbf_***";
                
                var sbf_***_ptr = Module.findExportByName(module, functionName);
                // var sbf_***_func = new NativeFunction(sbf_***_ptr, "bool", []);
                
                Interceptor.attach(sbf_***_ptr, {
                    onLeave: function(retval) {
                        // console.log("[*] retval sbf_***(): " + retval);
                        var newretval = ptr("0x0"); 
                        retval.replace(newretval);
                        console.log("[*] *** bypass");
                    }
                });
            } 
            catch(err) { 
                console.log("[!] Exception2: " + err.message); 
            } 
        } 
        else { 
            console.log("Objective-C Runtime is not available!"); 
        }
    }
}
"""


class JBBypass(Plugin):
    """ JBBypass is a plugin for bypass JB Detection (***) """

    def __init__(self, ns):
        """
            Creates a new instance of the plugin

            :param ns:
        """

        self.script_src = s
        # self.script_path = os.path.join(os.path.dirname(__file__), "script.js")

        implementation = {
            'meta': 'JB Detection bypass',
            'commands': {
                'info': {
                    'meta': 'Get the current Frida version',
                    'exec': self.bypass
                }
            }
        }

        super().__init__(__file__, ns, implementation)

        self.inject()

    def bypass(self, args: list):
        """
            
        """

        self.api.test()
        # print('Frida version: {0}'.format(v))


namespace = 'bypass'
plugin = JBBypass

```

Написание плагинов для objection: [https://github.com/sensepost/objection/wiki/Plugins](https://github.com/sensepost/objection/wiki/Plugins)

Больше плагинов смотри в примерах в коде: там есть все - и подгрузка своих классов, apk, jar и тд

Работа с задачами \(возможно, здесь можно закидывать свои скрипты\): [https://github.com/sensepost/objection/wiki/Working-with-Jobs](https://github.com/sensepost/objection/wiki/Working-with-Jobs)

## Примеры

### Запуск

Запуск: objection --gadget "&lt;app-id&gt;" explore

### Поиск и перехват методов

watching: [https://github.com/sensepost/objection/blob/master/objection/console/helpfiles/ios.hooking.watch.method.txt](https://github.com/sensepost/objection/blob/master/objection/console/helpfiles/ios.hooking.watch.method.txt)

Search:

```text
Поиск CNF*:
ios hooking search [classes|methods] CNF 
```

### Другое

Патчинг Split APKs [https://nickbloor.co.uk/2020/03/29/patching-android-split-apks/](https://nickbloor.co.uk/2020/03/29/patching-android-split-apks/)





