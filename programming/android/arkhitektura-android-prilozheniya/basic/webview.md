# WebView

## Intro

## Security

### Improper usage WebView

Есть два sandbox bypass: 

#### JS может вызывать Java код

```javascript
wv.addJavascriptInterface(new FileUtils(), “file”);
<script>
filename = ‘/data/data/com.Foudnstone/data.txt’;
file.write(filename, data, false);
</script>
```

#### Java код может вызывать JS

```java
String javascr = “javascript: var newscript=document.
createElement(\”script\”);”;
javascr += “newscript.src=\”http://www.foundstone.com\”;”;
javascr += “document.body.appendChild(newscript);”;
myWebView.loadUrl(javascr);
```

Что безопаснее WebView или Chrome Custom Tabs: [https://developer.chrome.com/multidevice/android/customtabs](https://developer.chrome.com/multidevice/android/customtabs)

