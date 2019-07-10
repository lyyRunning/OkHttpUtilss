# OkHttpUtils
最简单的okhttp封装，CallBack方法执行在UI线程。支持get请求，post请求，支持文件上传和下载。

## 使用方法：
代码很简单，只有三个Java文件，建议下载后将Java文件拷贝到工程中使用。

## 封装的功能有：
###### 一般的get请求
###### 一般的post请求
###### 一般的put请求
###### 一般的delete请求
###### 上传单个文件(包含进度)
###### 上传list集合文件
###### 上传map集合文件
###### 文件下载(包含进度)
###### 图片下载(实现了图片的压缩)

## 使用示例
### GET请求
    String url = "https://www.baidu.com/";
    OkhttpUtil.okHttpGet(url, new CallBackUtil.CallBackString() {
        @Override
        public void onFailure(Call call, Exception e) {}

        @Override
        public void onResponse(String response) {
            Toast.makeText(MainActivity.this,"Success",Toast.LENGTH_SHORT).show();
            Log.d("kwwl",response);
        }
    });
### POST请求
    String url = "https://www.baidu.com/";
    HashMap<String, String> paramsMap = new HashMap<>();
    paramsMap.put("title","title");
    OkhttpUtil.okHttpPost(url, paramsMap, new CallBackUtil.CallBackString() {
        @Override
        public void onFailure(Call call, Exception e) {

        }

        @Override
        public void onResponse(String response) {

        }
    });

### PUT请求
    String url = "https://www.baidu.com/";
    HashMap<String, String> paramsMap = new HashMap<>();
    paramsMap.put("title","title");
    OkhttpUtil.okHttpPut(url, paramsMap, new CallBackUtil.CallBackString() {
        @Override
        public void onFailure(Call call, Exception e) {

        }

        @Override
        public void onResponse(String response) {

        }
    });

### DELETE请求
    String url = "https://www.baidu.com/";
    HashMap<String, String> paramsMap = new HashMap<>();
    paramsMap.put("title","title");
    OkhttpUtil.okHttpDelete(url, paramsMap, new CallBackUtil.CallBackString() {
        @Override
        public void onFailure(Call call, Exception e) {

        }

        @Override
        public void onResponse(String response) {

        }
    });

### 上传文件
    File file = new File(Environment.getExternalStorageDirectory()+"/kwwl/abc.jpg");
    HashMap<String, String> paramsMap = new HashMap<>();
    paramsMap.put("title","title");
    OkhttpUtil.okHttpUploadFile(url, file, "image", OkhttpUtil.FILE_TYPE_IMAGE, paramsMap, new CallBackUtil.CallBackString() {
        @Override
        public void onFailure(Call call, Exception e) {

        }

        @Override
        public void onProgress(float progress, long total) {

        }

        @Override
        public void onResponse(String response) {

        }
    });

### 下载文件
    OkhttpUtil.okHttpDownloadFile("url", new CallBackUtil.CallBackFile("fileDir","fileName") {
        @Override
        public void onFailure(Call call, Exception e) {

        }

        @Override
        public void onProgress(float progress, long total) {
        }

        @Override
        public void onResponse(File response) {

        }
    });


### 添加拦截器

 // mOkHttpClient = new OkHttpClient();

        mOkHttpClient = new OkHttpClient().newBuilder()
                //10秒连接超时
                .connectTimeout(connectTimeout, TimeUnit.MINUTES)
                //10m秒写入超时
                .writeTimeout(writeTimeout, TimeUnit.MINUTES)
                //10秒读取超时
                .readTimeout(readTimeout, TimeUnit.MINUTES)
                .addInterceptor(new HttpHeaderInterceptor())//头部信息统一处理
                //.addInterceptor(new CommonParamsInterceptor())//公共参数统一处理
                .build();
### 拦截器类


public class HttpHeaderInterceptor implements Interceptor {

    @Override
    public Response intercept(@NonNull Chain chain) throws IOException {
        // 拦截请求，获取到该次请求的request
        Request request = chain.request();
        // 执行本次网络请求操作，返回response信息
        Response response = chain.proceed(request);
      /*  if (BuildConfig.DEBUG) {
            for (String key : request.headers().toMultimap().keySet()) {
                Log.e("LUO", "header: {" + key + " : " + request.headers().toMultimap().get(key) + "}");
            }
            Log.e("LUO", "url: " + request.url().uri().toString());
            ResponseBody responseBody = response.body();

            if (HttpHeaders.hasBody(response) && responseBody != null) {
                BufferedReader bufferedReader = new BufferedReader(new
                        InputStreamReader(responseBody.byteStream(), "utf-8"));
                String result;
                while ((result = bufferedReader.readLine()) != null) {
                    Log.e("LUO", "response: " + result);
                }
                // 测试代码,
                //responseBody.string();
            }
        }*/
        // 注意，这样写，等于重新创建Request，获取新的Response，避免在执行以上代码时，
        // 调用了responseBody.string()而不能在返回体中再次调用。
        return response.newBuilder().build();
    }

}













