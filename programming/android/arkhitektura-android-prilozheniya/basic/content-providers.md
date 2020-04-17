# Content Providers

Позволяет приложениям обмениваться данными, используя URI Scheme и БД.  
[https://github.com/nowsecure/secure-mobile-development/blob/master/en/android/implement-content-providers-carefully.md](https://github.com/nowsecure/secure-mobile-development/blob/master/en/android/implement-content-providers-carefully.md)

```markup
<provider
    android:authorities="com.zfr.intents.backup"
    android:name="androidx.core.content.FileProvider"
    android:enabled="true"
    android:exported="true"
    android:grantUriPermissions="true">
    <meta-data android:name="android.support.FILE_PROVIDER_PATHS" android:resource="@xml/backup_path"/>
    
</provider>
```



