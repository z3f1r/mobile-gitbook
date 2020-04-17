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

```markup
@xml/backup_path
<?xml version="1.0" encoding="utf-8"?>
<paths>
    <files-path name="backup" path="backup/"/>
</paths>
```

Херня с FileProvider нихера не работает: ограничение на exported + выдаваемые permissions работают, только пока живет контекст. Закрылся IntentService -&gt; отозвались permissions

