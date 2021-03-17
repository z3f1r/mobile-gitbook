# Android

## Ссылки

Детект библиотеки для пиннинга: [https://codeshare.frida.re/@akabe1/frida-multiple-unpinning/](https://codeshare.frida.re/@akabe1/frida-multiple-unpinning/)

universal script unpinning: [https://codeshare.frida.re/@pcipolloni/universal-android-ssl-pinning-bypass-with-frida/](https://codeshare.frida.re/@pcipolloni/universal-android-ssl-pinning-bypass-with-frida/)

Набор скриптов:   
[https://github.com/m0bilesecurity/Frida-Mobile-Scripts](https://github.com/m0bilesecurity/Frida-Mobile-Scripts)  
[https://github.com/LizhangHuang/FridaScript](https://github.com/LizhangHuang/FridaScript)  


Извлечение информации о Bluetooth: [https://github.com/k3170makan/FridaAndroidScripts/tree/master/bluecrawl](https://github.com/k3170makan/FridaAndroidScripts/tree/master/bluecrawl)

## Скрипты

## Wrapper

```javascript
setTimeout(
    function(){
        console.log("[+] Start");
        Java.perform(
            function(){
                try {
                    // Hook
                } catch(err) {
                    console.log("[-] Fail");
                }
            }
        );
    }
,3);
```

### PhoneGap & Outsystem ssl pinning bypass

src: [https://github.com/clviper/android/blob/master/pinning.js](https://github.com/clviper/android/blob/master/pinning.js)

### OkHttp3 SSL Pinning bypass

```javascript
setTimeout(
    function() {
        console.log("[+] Start");
        Java.perform(function() {
            try {
                console.log("[+] Start 2");
                var OkHttpClient_Builder = Java.use('okhttp3.OkHttpClient$Builder');
                console.log("[+] OkHTTP 3.x Found");

                // Disable adding CertificatePinner Objects
                
                OkHttpClient_Builder.certificatePinner.overload('okhttp3.CertificatePinner').implementation = function(certificatePinner) {
                    console.log("[+] OkHTTP 3.x certificatePinner() called. Not throwing an exception.");
                    return this;
                };

                OkHttpClient_Builder.hostnameVerifier.overload('javax.net.ssl.HostnameVerifier').implementation = function(hostnameVerifier) {
                    console.log("[+] OkHTTP 3.x hostnameVerifier() called. Not throwing an exception.");
                    return this;
                };


                // Disable CertificatePinner checks
                
                OkHttpClient_Builder.certificatePinner.overload('okhttp3.CertificatePinner').implementation = function(certificatePinner) {
                    console.log("[+] OkHTTP 3.x certificatePinner() called. Not throwing an exception.");
                    return this;
                };

                var CertificatePinner = Java.use('okhttp3.CertificatePinner');
                CertificatePinner.check.overload('java.lang.String', 'java.util.List').implementation = function(s, l) {
                    console.log("[+] Unpinning Type 1: " + s);
                    return true;
                };

                CertificatePinner.check.overload('java.lang.String', 'java.security.cert.Certificate').implementation = function(s, cert) {
                    console.log("[+] Unpinning Type 2: " + s);
                    return true;
                }

                CertificatePinner.check.overload('java.lang.String', 'javax.security.cert.Certificate').implementation = function(s, cert) {
                    console.log("[+] Unpinning Type 3: " + s);
                    return true;
                }
            } catch (err) {
                console.log("[-] OkHTTP 3.x Not Found")
            }
        })
    }, 
3);
```







