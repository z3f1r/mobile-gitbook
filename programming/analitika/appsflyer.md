# AppsFlyer

Пример

```java
import com.appsflyer.AppsFlyerConversionListener;
import com.appsflyer.AppsFlyerLib;

static  {
    APP_FL_KEY = "i7EvqrSvMEig9UNGJLX2Me";
}

AppsFlyerLib.getInstance().init(APP_FL_KEY, appsFlyerConversionListener, getApplicationContext()).getInstance().startTracking(this);
```

