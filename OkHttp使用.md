##  OkHttp使用

####  1.导包

```implementation 'com.squareup.okhttp3:okhttp:5.0.0-alpha.2'```

```compile 'com.squareup.okio:okio:2.0.0'```(OkHttp内部依赖)

PS：记得添加网络权限

```<uses-permission android:name="android.permission.INTERNET"/>```

####  2.新建Client对象

``````java
OkHttpClient client = new OkHttpClient();
``````

####  3.传入参数

#####  Get请求

```java
Request request=new Request.Builder().url(netUrl).get().build();
	//建立Request对象，传入的netUrl即为目标url
Call call = client.newCall(request);
	//通过Client的newCall方法将request传入，构造一个Call对象
```

#####  Post请求

post请求与get不同，post方式中的Request需要传递一个带有上传数据的RequestBody作为参数，故先要对上传的文件作封装处理。



常用的有`RequestBody`、`FormBody`、`MultipartBody`

- RequestBody--可传字符串、文件

  ```java
  public static final MediaType TYPE = MediaType.parse("text/plain; charset=utf-8");
  	//解析一会需要传入的MediaType参数,可替换为多种类型
  	//获取text对象（略）
  RequestBody body = RequestBody.create(TYPE,text);
  	//创建Requestbody
  
  Request request = new Request.Builder().url(netUrl).post(body).build();
  	//传入body
  ```

- FormBody--用于提交表单（键值对）

  ```jav
  FormBody.Builder builder = new FormBody.Builder();
  builder.add("test",1);
  builder.add("hhh",2);
  FormBody body = builder.bulid();
  
  Request request = new Request.Builder().url(netUrl).post(body).build();
  ```

- MultipartBody--文件、键值对

  ```java
  RequestBody muiltipartBody = new MultipartBody.Builder()
          .setType(MultipartBody.FORM)
      	//若提交表单，要添加这一句
          .addFormDataPart("username", "admin")
          .addFormDataPart("password", "admin")
      	//可添加表单
          .addFormDataPart("myfile", "1.png", RequestBody.create(MediaType.parse("application/octet-stream"), file))
      	//可添加文件，（"键",本地文件名,RequestBody包含TYPE和			file对象）
          .build();
  
  Request request = new Request.Builder().url(netUrl).post(muiltipartBody).build();

####  4.回调结果

通过RequestBody和Client构建Call对象

```java
Call call=client.newCall(request);
```

- 同步（不推荐）

  ```jav
  Response response = call.execute()
  ```

  

- 异步

  ```java
  call.enqueue(new Callback() {
              @Override
              public void onFailure(Call call, IOException e) {
                  //请求失败的处理
              }
              @Override
              public void onResponse(Call call, Response response) throws IOException { 
                  //请求成功的处理
              }
          });
  ```

  