# Examples

Интеграция на примере Android: [https://support.appsflyer.com/hc/ru/articles/207032126](https://support.appsflyer.com/hc/ru/articles/207032126)

Examples apps: [https://github.com/AppsFlyerSDK/SampleApp-Examples](https://github.com/AppsFlyerSDK/SampleApp-Examples)

Пример

```java
import com.appsflyer.AppsFlyerConversionListener;
import com.appsflyer.AppsFlyerLib;

static  {
    APP_FL_KEY = "i7EvqrSvMEig9UNGJLX111";
}

AppsFlyerLib.getInstance().init(APP_FL_KEY, appsFlyerConversionListener, getApplicationContext()).getInstance().startTracking(this);
```



