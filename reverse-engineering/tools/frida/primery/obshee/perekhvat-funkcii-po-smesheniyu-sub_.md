# Перехват функции по смещению \(sub\_\*\)

```javascript
var baseAddress = Module.findBaseAddress('FMCore');
var sub_7224 = baseAddress.add(0x7224);  // sub_7224
 
Interceptor.attach(sub_7224, {
    onEnter: function (args) {
        console.log('Test sub_7224 (= sbf_wasabit): ' + args[0]);
    }
});
```

