##  Retrofit使用



####  1.添加依赖

```implementation 'com.squareup.retrofit2:retrofit:2.9.0'```

因为retrofit是依赖于OkHttp的，故也要添加OkHttp依赖。

```implementation 'com.squareup.okhttp3:okhttp:5.0.0-alpha.2'```

####  2.添加网络权限

```<uses-permission android:name="android.permission.INTERNET"/>```

####  3.创建接收数据类

```java
public class Reception {
    ...
    // 根据返回数据的格式和数据解析方式（Json、XML等）定义
        }
```



####  4.创建用于描述网络请求的接口

#####  一、设定网络请求方法

```java
public interface GetRequest{
    @GET("url")
    //一般传入在此传入url的相对路径，在创建Retrofit时设置域名
    //此外还有@POST\@PUT\@DELETE\@PATH\@HEAD\@OPTIONS\\@HTTP
    Call<*>  getCall();
    // @GET注解的作用:采用Get方法发送网络请求
    // getCall()：接收网络请求数据的方法
    // 其中返回类型为Call<*>，*是接收数据的类
```

除@HTTP型之外，其余全部传入URL

@HTTP型注解：

```java
public interface GetRequest{
    /**
     * method：网络请求的方法（区分大小写）
     * path：网络请求地址路径
     * hasBody：是否有请求体
     */
    @HTTP(method = "GET", path = "blog/{id}", hasBody = false)
    Call<ResponseBody> getCall(@Path("id") int id);
    // {id} 表示是一个变量
    // method 的值 retrofit 不会做处理，所以要自行保证准确
}
```

- 网络请求的完整URL有如下几种整合方式：

  ![p1](D:\MYGITS\camp-daily\p1.png)

- P.S.:http中URL的格式：

  ```http://www.aspxfans.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name```

从上面的URL可以看出，一个完整的URL包括以下几部分：
1.协议部分：该URL的协议部分为“http：”，这代表网页使用的是HTTP协议。在Internet中可以使用多种协议，如HTTP，FTP等等本例中使用的是HTTP协议。在"HTTP"后面的“//”为分隔符

2.域名部分：该URL的域名部分为```www.aspxfans.com```。一个URL中，也可以使用IP地址作为域名使用

3.端口部分：跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。端口不是一个URL必须的部分，如果省略端口部分，将采用默认端口

4.虚拟目录部分：从域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。虚拟目录也不是一个URL必须的部分。本例中的虚拟目录是“/news/”

5.文件名部分：从域名后的最后一个“/”开始到“？”为止，是文件名部分，如果没有“?”,则是从域名后的最后一个“/”开始到“#”为止，是文件部分，如果没有“？”和“#”，那么从域名后的最后一个“/”开始到结束，都是文件名部分。本例中的文件名是“index.asp”。文件名部分也不是一个URL必须的部分，如果省略该部分，则使用默认的文件名

6.锚部分：从“#”开始到最后，都是锚部分。本例中的锚部分是“name”。锚部分也不是一个URL必须的部分

7.参数部分：从“？”开始到“#”为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为“boardID=5&ID=24618&page=1”。参数可以允许有多个参数，参数与参数之间用“&”作为分隔符。

#####  二、标记上传文件类型

1. @FormUrlEncoded

作用：表示发送form-encoded的数据
每个键值对需要用@Filed来注解键名，随后的对象需要提供值。

2.  @Multipart

作用：表示发送form-encoded的数据（适用于 有文件 上传的场景）
每个键值对需要用@Part来注解键名，随后的对象需要提供值。

```java
public interface PostRequest{
        @POST("/form")
        @FormUrlEncoded
        Call<ResponseBody> testFormUrlEncoded(@Field("username") String name, @Field("age") int age);
//----------------------------------------------------------
        @POST("/form")
        @Multipart
        Call<ResponseBody> testFileUpload(@Part("name") RequestBody name, @Part("age") RequestBody age, @Part MultipartBody.Part file);
}
```

#####  三、网络请求参数

//暂略

#####  汇总

![p2](D:\MYGITS\camp-daily\p2.jpg)

####  4.创建Retrofit实例

```java
Retrofit retrofit = new Retrofit.Builder()
.baseUrl("http://...") // 设置网络请求的Url地址
.addConverterFactory(...) //数据解析器
.addCallAdapterFactory(...) //网络请求适配器
.build();
```

- 数据解析器&网络请求适配器按需使用，此处不再赘述

####  5.创建网络请求接口实例

```java
GetRequest request = retrofit.create(GetRequest.class);

Call<Reception> call = request.getCall();
```

```java
PostRequest request = retrofit.creat(PostRequest.class);

Call<ResponseBody> call = retrofit.testFormUrlEncoded("wl",19);
```

####  6.通过Call发送请求并处理回调

```java
call.enqueue(new Callback<Translation>() {
            //请求成功时回调
            @Override
            public void onResponse(Call<Reception> call, Response<Translation> response) {
                response.body();//获取数据内容
            }

            //请求失败时候的回调
            @Override
            public void onFailure(Call<Reception> call, Throwable throwable) {
                
            }
        });
```

