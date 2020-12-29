# Перехват инструкции по смещению

```javascript
var baseAddress = Module.findBaseAddress('FMCore');
var addr = baseAddress.add(0x723c); // return from sub_7224
 
Interceptor.attach(addr, function(args) {
    console.log("some trace instr: " + this.context.x0);  // любой регистр: r0-r15, x0-x15, ...
});
```

