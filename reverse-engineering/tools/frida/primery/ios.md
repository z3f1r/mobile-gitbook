# iOS

## Alamofire SSL Pinning Disable

```javascript
// frida -U -l afnetwork_sslbypass.js --no-pause -f my.test.app
 
console.log("[*] Started: SSL Unpinning");
if (ObjC.available) {
    /* AFNetwork */
    if (ObjC.classes.AFSecurityPolicy) {
        Interceptor.attach(ObjC.classes.AFSecurityPolicy['- setSSLPinningMode:'].implementation, {
            onEnter: function (args) {
                send("[*] Replacing AFSecurityPolicy setSSLPinningMode = 0 was " + args[2]);
                args[2] = ptr('0x0');
            }
        });
        Interceptor.attach(ObjC.classes.AFSecurityPolicy['- setAllowInvalidCertificates:'].implementation, {
            onEnter: function (args) {
                send("[*] Replacing AFSecurityPolicy setAllowInvalidCertificates = 1 was " + args[2]);
                args[2] = ptr('0x1');
            }
        });   
    }
}
else
{
    console.log("Objective-C Runtime is not available!");
}
console.log("[*] Completed: SSL Unpinning");
```

## ASLR Bypass: Получение адреса функции

```javascript
var modules_dict = {};

// save module addresses for bypass ASLR on iOS
function get_modules_dict() {
    var native_modules = Process.enumerateModulesSync();
    var i;
    for (i = 0; i < native_modules.length; i++)
    {
        modules_dict[native_modules[i].path] = {name: native_modules[i].name, path: native_modules[i].path, base: parseInt(native_modules[i].base.toString(), 16), size: parseInt(native_modules[i].size, 10), slide: -1, min: -1, max: -1};
    }

    // unsigned long _dyld_image_count();
    var dyldImageCountPtr = Module.findExportByName("libSystem.B.dylib", "_dyld_image_count");
    var dyldImageCount = new NativeFunction(dyldImageCountPtr, 'int', []);

    // unsigned long _dyld_get_image_vmaddr_slide(unsigned long image_index);
    var dyldImageVMAddrSlidePtr = Module.findExportByName("libSystem.B.dylib", "_dyld_get_image_vmaddr_slide");
    var dyldImageVMAddrSlide = new NativeFunction(dyldImageVMAddrSlidePtr, 'ulong', ['ulong']);

    //pointer _dyld_get_image_name(ulong);
    var dyldImageNamePtr = Module.findExportByName("libSystem.B.dylib", "_dyld_get_image_name");
    var dyldImageName = new NativeFunction(dyldImageNamePtr, 'pointer', ['ulong']);

    var count = dyldImageCount();
    for (i=0; i<count; i++)
    {
        var imgName = Memory.readCString(dyldImageName(i));
        var imgAddr = dyldImageVMAddrSlide(i);
        modules_dict[imgName]['slide'] = parseInt(imgAddr.toString(), 10);
        modules_dict[imgName]['min'] = modules_dict[imgName]['base'];
        modules_dict[imgName]['max'] = modules_dict[imgName]['base'] + modules_dict[imgName]['size'];
    }
    return modules_dict;
}
// Get original address for iOS binary
function find_module_offset_by_addr(orig_addr, lookup_func_name)
{
    var mp;
    for (mp in modules_dict)
    {
        if (orig_addr >= modules_dict[mp]['min'] && orig_addr <= modules_dict[mp]['max'])
        {
            var func_name = '';
            if (lookup_func_name === true)
            {
                func_name = " " + DebugSymbol.fromAddress(orig_addr)['name'];
            }
            var dbg_str = " (B:" + modules_dict[mp]['base'] + "|S:" + modules_dict[mp]['slide'] + "|O:" + orig_addr + ")";
            return orig_addr.sub(modules_dict[mp]['slide']) + " " + modules_dict[mp]['name'] + "+" + (orig_addr - modules_dict[mp]['min']).toString(16) + func_name; // + dbg_str;
        }
    }
    return "[DBGS]" + DebugSymbol.fromAddress(orig_addr);
    //return "UNKNOWN!" + orig_addr
}

/*
* Example usage
*/

if (ObjC.available) {	

   var module = "SDSearch";
   var functionName = "sbf_wasabit";

   var sbf_wasabit_ptr = Module.findExportByName(module, functionName);

   modules_dict = get_modules_dict();

   console.log("Wasabit module offset: " + find_module_offset_by_addr(sbf_wasabit_ptr)); // По полученному адресу можно осуществлять поиск в IDA
}
```

