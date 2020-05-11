# Общие команды \(JS API\)

## Общие функции

```text
hexdump(target[, options]) - генерация хексдампа по ArrayBuffer или указателю (NativePointer) с опциями
    например: hexdump(buf, {offset: 0, length: 64, header: true, ansi: true})
int64(v): краткое объявление для new Int64(v)
uint64(v): то же для new UInt64(v)
ptr(v): то же для new NativePointer(s)
NULL: то же для ptr("0")
recv([type, ]callback) - получения ответа от нашего Frida-based приложения.
send(message[, data]) - отправка сообщения приложению (дб сериализовано в JSON)
```

## Вывод в консоль

```javascript
console.log(line)
console.warn(line) 
console.error(line)
```

## Работа с памятью

```text
Указатель: 
Memory.readPointer(address) - читает указатель по адресу address и возвращает NativePointer
Числа:
Memory.readS8(address), 
Memory.readU8(address), 
Memory.readS16(address), 
Memory.readU16(address), 
Memory.readS32(address), 
Memory.readU32(address), 
Memory.readShort(address), 
Memory.readUShort(address), 
Memory.readInt(address), 
Memory.readUInt(address), 
Memory.readFloat(address), 
Memory.readDouble(address)
Memory.readS64(address), 
Memory.readU64(address), 
Memory.readLong(address), 
Memory.readULong(address),
Массивы:
Memory.readByteArray(address, length) - возвращает ArrayBuffer
Строки:
Memory.readCString(address[, size = -1])
Memory.readUtf8String(address[, size = -1])
Memory.readUtf16String(address[, length = -1])
Memory.readAnsiString(address[, size = -1])
```

## Работа с числами

```text
new Int64(v) = int64(v) - делает число из JSNumber, или строки с числом, или хекса, начинающегося с '0x'
Методы Int64, UInt64:
num.add(rhs) = num + rhs
Другие методы: sub, and, or, xor, shr, shl, compare
Конвертирование: toNumber(), toString(), toInt32()
```

## Работа с указателями

```text
new NativePointer(s) = ptr(s)
isNull()
add, sub, and, or, xor, shr, shl
equals(num), compare(num)
toInt32()
toString([radix = 16])
toMatchPattern(): returns a string containing a Memory.scan()-compatible match pattern for this pointer’s raw value

NativeFunction() - !!!! Можно вызывать свои функции?!!!!!
```

## Работа с классами и объектами

### iOS

```text
Получение объекта ios:
var webSocket = new ObjC.Object(args[2]);
Структуры:
    6.1 ClassName:
console.log("Type of args[2] -> " + new ObjC.Object(args[2]).$className)
Другие свойства:
$kind = {instance, class, meta-class}
$super - super-class method impl
$superClass - super-class as ObjC.Object instance
$class - class this object as ...
$methods - методы текущего и родительского классов
$ownMethods - методы текущего класса
$ivars - название полей
    6.2 NSData -> String
var data = new ObjC.Object(args[2]);
Memory.readUtf8String(data.bytes(), data.length());
    6.3 NSData -> Binary Data
var data = new ObjC.Object(args[2]);
Memory.readByteArray(data.bytes(), data.length());
    6.4 Проходим по NSArray
var array = new ObjC.Object(args[2]);
var count = array.count().valueOf();
for (var i = 0; i !== count; i++) {
  var element = array.objectAtIndex_(i);
}
    6.5 Проходим по NSDictionary
var dict = new ObjC.Object(args[2]);
var enumerator = dict.keyEnumerator();
var key;
while ((key = enumerator.nextObject()) !== null) {
    var value = dict.objectForKey_(key);
}
6.6 Reading a struct
If args[0] is a pointer to a struct, and let’s say you want to read the uint32 at offset 4, you can do it as shown below:

Memory.readU32(args[0].add(4));


Доступ к приватным полям
for (k in objectClass.$ivars) {
    log("Test " + k);
    // out:
    // Test _test_property
}
prop = objectClass.$ivars._test_property 
```

### Android

```text
Список методов и полей JAVA:
var _j_GoogleFit_hmq = Java.use("hmq");
var objDataHMQ = Java.cast(data, _j_GoogleFit_hmq); // hmq
java_msg(Object.getOwnPropertyNames(objDataHMQ.__proto__).join(' '));

Что б получить доступ к приватному полю, используем свойство value:
class A {
	private int b;
}
var classA = Java.use("A");
var objClassA = Java.cast(pointer, classA);
java_msg(Object.getOwnPropertyNames(objClassA.__proto__).join(' ')); -> посмотрели какие методы и поля у класса
var field_b = objClassA._b.value; -> а это уже наше поле


Вывести array как hex-строку: byte[3412341241]:
var ret = this.fn();
var buffer = Java.array('byte', ret);
console.log(buffer.length);
var result = "";
for(var i = 0; i < buffer.length; ++i){
    result += ('0' + (buffer[i] & 0xff).toString(16)).slice(-2);
}
console.log(result);
```

