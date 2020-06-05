# WebView

## WKWebView

```swift
webView = WKWebView(...)
webView.uiDelegate = self  // должен реализовывать метод viewDidLoad()

...
override func viewDidLoad() {
    super.viewDidLoad()
    
    let myURL = URL(string:"https://www.apple.com")
    let myRequest = URLRequest(url: myURL!)
    webView.load(myRequest)
}}
```

