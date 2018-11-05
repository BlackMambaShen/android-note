# android-note
安卓学习笔记
ative与html5:
1.设置允许执行js脚本

2.添加通信接口

3.JS调用android

4.android调用JS


安卓代码
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private WebView webView;
    private Button btn;
    private WebAppInterface appInterface;
    @SuppressLint("JavascriptInterface")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
         webView = (WebView)findViewById(R.id.webView);
        btn = (Button)findViewById(R.id.btn);
        btn.setOnClickListener(this);
         webView.loadUrl("file:///android_asset/index1.html");
         webView.getSettings().setJavaScriptEnabled(true);
         appInterface=new WebAppInterface(this);
         webView.addJavascriptInterface(appInterface,"app");
    }

    @Override
    public void onClick(View v) {
        appInterface.showName("菜鸟窝");
    }

    class WebAppInterface{
        private Context context;

        public WebAppInterface(Context context){
            this.context=context;
        }
        @JavascriptInterface
        public void sayHello(String name){
            Toast.makeText(context, "name:"+name, Toast.LENGTH_SHORT).show();
        }

        public void showName(final String name){
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    webView.loadUrl("javascript:showName('"+name+"')");
                }
            });
        }
    }
}


布局文件：
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <Button
        android:text="调用js"
        android:id="@+id/btn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</LinearLayout>



h5代码：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML5 测试</title>
    <style type="text/css">
        .input{
            width: 90%;
            height:30px;
        }

        .button{
            width:60%;
            height: 20px;
            background: #ff00ff;
        }
    </style>

    <script type="text/javascript">
        function sayhello() {
            var name=document.getElementById("txtName").value;
            app.sayHello(name);
        }

        function showName(name) {
            document.getElementById("txtName").value=name;
        }
    </script>
</head>
<body>
<input id="txtName"
class="input" >
<br/>
<hr>
<button class="button" onclick="sayhello()">say hello</button>
</body>
</html>
