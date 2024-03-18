하드웨어 가속
[blog](heegs.tistory.com)


WebView는 안드로이드 앱 내에서 웹 콘텐츠를 표시하고 웹 페이지와 상호 작용하기 위한 컴포넌트. 쉽게 말해, 웹 페이지를 앱 내부에서 보여주는 기능 제공. 웹 브라우저를 앱에 통합하거나, 앱 내에서 웹 기반의 컨텐츠를 표시하고 상호 작용하는 데 사용.



android에서 webView를 표현하려면,
XML 레이아웃에서 webView 추가하고 activity나 fragment에서 webView 로드하여 설정 및 이벤트 처리
```kotlin
<WebView android:id="@+id/webView" android:layout_width="match_parent" android:layout_height="match_parent" />


val webView: WebView = findViewById(R.id.webView) 
webView.settings.javaScriptEnabled = true // JavaScript를 사용하려면 활성화 
// URL 로드 
webView.loadUrl("https://www.example.com") 
// HTML 문자열 로드 
val htmlString = "<html><body><h1>Hello, WebView</h1></body></html>" webView.loadData(htmlString, "text/html", "UTF-8")


webView.webViewClient = WebViewClient() // WebViewClient 설정 
webView.webChromeClient = WebChromeClient() // WebChromeClient 설정 
// WebViewClient를 통해 페이지 로딩 이벤트 처리 
webView.webViewClient = object : WebViewClient() { 
	override fun onPageStarted(view: WebView?, url: String?, favicon: Bitmap?) { 
		// 페이지 로딩이 시작될 때 호출 
	} 
	override fun onPageFinished(view: WebView?, url: String?) {
		// 페이지 로딩이 완료될 때 호출 
	} 
}
```


compose는 선언적 UI이므로 'AndroidView'를 통해 기존 View를 사용할 수 있음.
```kotlin
class MainActivity : ComponentActivity() { 
	override fun onCreate(savedInstanceState: Bundle?) { 
		super.onCreate(savedInstanceState) 
		setContent { 
			MyWebView("https://www.example.com") 
		} 
	} 
} 


@Composable 
fun MyWebView(url: String) { 
	AndroidView( 
	factory = { context -> 
		WebView(context).apply { 
			settings.javaScriptEnabled = true 
			loadUrl(url) 
		} 
	}, 
	modifier = Modifier.fillMaxSize() 
	) 
}

```

