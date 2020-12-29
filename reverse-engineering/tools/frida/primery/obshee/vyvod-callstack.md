# Вывод Callstack

## iOS

```javascript
console.log("ObjC callstack:\\r\\n" + ObjC.classes.NSThread.callStackSymbols().toString());
```

## Java

```javascript
var _java_Log = Java.use("android.util.Log");
var _java_Exception = Java.use("java.lang.Exception");
 
function java_print_callstack(name)
{
    var callstack = _java_Log.getStackTraceString(_java_Exception.$new());
    if (typeof name != "undefined")
    {
        java_msg(name + ' callstack = ' + callstack);
    }
    else
    {
        java_msg('... callstack = ' + callstack);
    }
}
```

## C

```javascript
Thread.backtrace(this.context, Backtracer.ACCURATE).map(function (addr){return find_module_offset_by_addr(addr, true);}).join("\r\n") + "\r\n";
```

