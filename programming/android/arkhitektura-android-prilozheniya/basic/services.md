# Services

IntentService - запускается в отдельном потоке \(=&gt; позволяет делать сетевые запросы\)

Service - работает в главном потоке

### Intro

Используются для обработки информации в background Как BroadcastReceivers и Activities, Services могут быть вызваны внешними приложениями =&gt; должны быть также защищены permissions и флагами export.

### Example

```markup
<permission android:name="com.example.mypermission" 
            android:label="my_permission" 
            android:protectionLevel="dangerous">
</permission>
<service android:name="com.example.MyService" 
         android:permission="com.example.mypermission">
    <intent-filter>
         <action android:name="com.example.MY_ACTION" />
    </intent-filter>
</service>
```

