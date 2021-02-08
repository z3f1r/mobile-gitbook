# Примеры уязвимостей

## Примеры уязвимостей в Swift

### Client Side Injection

Все данные, передаваемые от пользователя, должны проходить проверку на соответствие ожидаемому формату, при их обработке управляющие конструкции необходимо экранировать.

Ниже показан пример встраивания HTML-кода через форматную строку и его отображение в компоненте WKWebView.

```swift
let view = WKWebView()
let injUserData = "<strong>123</strong>"
view.loadHTMLString("Test \(injUserData) string", baseURL: nil)
// view.load(request)
return view
```

В общем случае, инъекция может произойти подобным образом не только в HTML-страницы, но и в запросы к серверу в JSON- , XML- структуры, в любые другие форматы хранения и обработки информации. А также не только из полей ввода в UI, но такие данные могут прийти при обработке Universal Links, открытии файлов и изображений пользователя, через механизм Share и тд.

### Правильное использование биометрии

В iOS для локальной аутентификаци \(Passcode, TouchID, FaceID\) на устройстве используется `Local Authentication Framework` и `Security.framework`.

Рассмотрим аутентификацию на примере с TouchID. Разработчики могут ее задействовать с помощью функции `evaluatePolicy` класса `LAContext`. Функция `evaluatePolicy` возвращает булевое значение, определяющее прошел ли пользователь аутентификацию или нет.

```swift
let context = LAContext()
var error: NSError?

guard context.canEvaluatePolicy(.deviceOwnerAuthentication, error: &error) else {
    // Could not evaluate policy; look at error and present an appropriate message to user
}

context.evaluatePolicy(.deviceOwnerAuthentication, localizedReason: "Please, pass authorization to enter this area") { success, evaluationError in
    guard success else {
        // User did not authenticate successfully, look at evaluationError and take appropriate action
    }

    // User authenticated successfully, take appropriate action
}
```

Проверка, опирающаяся только на результат результат булевой функции и не предполагающая какую-то последующую обработку данных \(как в примере выше\), может быть обойдена в некоторых случаях атакующим путем использования патчей или инструментации \(внедрение в уже запущенный процесс своего кода\).

Корректнее использовать для локальной аутентификации на устройстве сервис Keychain. При создании записи в Keychain можно с помощью атрибута SecAccessControl указать например, что значение записи можно получить только если на устройстве включен вход по Passcode \(`kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly`\) и доступ разрешен после прохождения TouchID \(`SecAccessControlCreateFlags.biometryCurrentSet`\):

```swift
guard let accessControl = SecAccessControlCreateWithFlags(kCFAllocatorDefault, kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly,    SecAccessControlCreateFlags.biometryCurrentSet, &error) else {
    // ошибка
    return
}

var query: [String: Any] = [:]
query[kSecClass as String] = kSecClassGenericPassword
...
query[kSecAttrAccessControl as String] = accessControl

let status = SecItemAdd(query as CFDictionary, nil)
```

Затем, при обращении к этой записи, у пользователя будет запрошена аутентификация. При успешном ее прохождении будет возвращено значение записи из Keychain, которое можно будет использовать, например, для расшифрования сессии и пользователя и продолжения работы приложения, в целом.

```swift
var queryResult: AnyObject?
let status = withUnsafeMutablePointer(to: &queryResult) {
    SecItemCopyMatching(query as CFDictionary, UnsafeMutablePointer($0))
}

if status == noErr {
    let someSecretValue = String(data: queryResult as! Data, encoding: .utf8)!
    // используем someSecretValue
} else {
    // авторизация не выполнена
}
```

### Про Access Control Flags в работе с записями в Keychain

Как следует из примера выше, с точки зрения безопасности лучше использовать метод локальной аутентификации на основе Keychain сервиса. Вам следует:

* Проверить, что все процессы, обрабатывающие важную информацию \(например, подтверждение платежных транзаций пользователя\), защищены этим методом
* Проверить, что флаги управления доступом установлены \(они гарантируют, что записи в Keychain могут быть разблокированы только путем аутентификации пользователя\). Это можно сделать с помощью одного из следующих флагов:
  * `kSecAccessControlBiometryCurrentSet` — требуется вход по TouchID или FaceID. При добавлении нового отпечатка доступ к записи получить не удасться.
  * `kSecAccessControlBiometryAny` — требуется вход по TouchID или FaceID. Доступ к полю сохраняется при добавлении новых отпечатков. Этот метод слабее `kSecAccessControlBiometryCurrentSet`. С точки зрения безопасности, при добавлении новых отпечатков, лучше уведомить об этом пользователя и заставить его повторно авторизоваться в вашем приложении.
  * `kSecAccessControlUserPresence` — вход по Passcode, если вход по TouchID или FaceID невозможен. Этот метод слабее `kSecAccessControlBiometryAny`, и с точки зрения безопасности не рекомендуется \(так как код легче получить, нежели пройти биометрию\). Однако, с точки зрения удобства для пользователя он предпочтителен.
* Проверить, что при создании записей в Keychain, для которых предполагается доступ по биометрии, был указан один из флагов — `kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly` или `kSecAttrAccessibleWhenPasscodeSet`

### Sensitive Data In Memory

Часто возникает необходимость обрабатывать и передавать конфиденциальную и иную \(пароли, токены, куки..\) информацию через память процесса. Рекомендуется, чтобы ваше приложение создавало как можно меньше копий этих данных в течение как можно меньшего времени, а по окончании работы удаляло их из памяти \(например, обнуляя значения, затирая нулями и тп\). Другими словами, вам нужна централизованная обработка конфиденциальных данных на основе примитивных и изменяемых структур данных.

В языке Swift не так много типов, которые предоставляют прямой доступ к памяти. К таким типам относятся простые типы — char, int и тп, а так же коллекции — Array, Set и Dictionary — над этими простыми типами.

Избегайте типов данных Swift, отличных от коллекций, независимо от того, считаются ли они изменяемыми. Многие типы данных Swift хранят свои данные по значению, а не по ссылке. Хотя это позволяет изменять память, выделенную для простых типов, таких как char и int, обработка сложного типа, такого как String по значению, включает в себя скрытый слой объектов, структур или примитивных массивов, память которых не может быть напрямую доступна или изменена.

Например, многие думают, что следующее приводит к изменяемой строке в Swift, но на самом деле это пример переменной, сложное значение которой может быть скопировано, а не изменено:

```swift
var str1 = "Goodbye"              // "Goodbye", base address:            0x0001039e8dd0
str1.append(" ")                  // "Goodbye ", base address:            0x608000064ae0
str1.append("cruel world!")       // "Goodbye cruel world", base address: 0x6080000338a0
str1.removeAll()                  // "", base address                     0x00010bd66180
```

Обратите внимание, что базовый адрес базового значения изменяется с каждой строковой операцией. Вот проблема: чтобы безопасно стереть конфиденциальную информацию из памяти, мы не хотим просто изменять значение переменной; мы хотим изменить фактическое содержимое памяти, выделенной для текущего значения. Swift не предлагает такой функции.

