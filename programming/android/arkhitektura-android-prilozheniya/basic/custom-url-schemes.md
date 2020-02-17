# Custom URL Schemes

```text
Example (Android Manifest):

<activity android:exported="true" android:name=".RedirectUriReceiverActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <category android:name="android.intent.category.BROWSABLE"/>
        <data android:scheme="myapp"/>
    </intent-filter>
</activity>

myapp://path/to/what/i/want?keyOne=valueOne&keyTwo=valueTwo
```

