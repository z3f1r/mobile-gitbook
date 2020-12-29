# Перехват функции по имени

```javascript
var module = "SDSearch";
var functionName = "sbf_wasabit";
var sbf_wasabit_ptr = Module.findExportByName(module, functionName);
Interceptor.attach(sbf_wasabit_ptr, {
    onLeave: function(retval) {
        var newretval = ptr("0x0");
        retval.replace(newretval);
        console.log("[+] Completed: Wasabit bypass");
    }
});
```

