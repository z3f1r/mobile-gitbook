# objection

## Info

Тулза над фридой. Умеет куча всего.

## Написание плагинов

Структура: 

![](../../.gitbook/assets/snimok-ekrana-2020-06-17-v-14.39.49.png)

Запуск:

```text
objection --gadget "ru.sberbank.sberfriendefs" explore -P "/Users/**/Work/pentest/projects/**/scripts" -s "plugin bypass info"
```

-P - указать папку с плагинами  
-s - запустить плагин во время запуска приложения

Пример плагина \(файл \_\_init\_\_.py\)

```python
__description__ = "SberFriend: JB Bypass (Wasabit)"

from objection.utils.plugin import Plugin

s = """
rpc.exports = {
    wasabit: function() {
        console.log("[+] Jailbreak Detection Bypass"); 
        if (ObjC.available) {
            try {  
                var module = "SDSearch";  // finded by frida-trace -U -f ru.sberbank.sbfriendefs -i "sbf_wasabit"
                
                var functionName = "sbf_wasabit";
                
                var sbf_wasabit_ptr = Module.findExportByName(module, functionName);
                // var sbf_wasabit_func = new NativeFunction(sbf_wasabit_ptr, "bool", []);
                
                Interceptor.attach(sbf_wasabit_ptr, {
                    onLeave: function(retval) {
                        // console.log("[*] retval sbf_wasabit(): " + retval);
                        var newretval = ptr("0x0"); 
                        retval.replace(newretval);
                        console.log("[*] Wasabit bypass");
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
    """ JBBypass is a plugin for bypass JB Detection (wasabit) """

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

        self.api.wasabit()
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





