## 集成LinkedMe

tip: **我的需求仅仅是从浏览器唤醒APP,并没有做深度链接**

[LinkMe官网](https://www.linkedme.cc/)

[github](https://codeload.github.com/WFC-LinkedME/LinkedME-iOS-Deep-Linking-Demo/zip/master)

1.注册账号

2.添加应用 && 应用配置

![image.png](https://upload-images.jianshu.io/upload_images/1419035-c8c8e4ca62f6ee2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)


![image.png](https://upload-images.jianshu.io/upload_images/1419035-9846dbcbc4b717b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

![image.png](https://upload-images.jianshu.io/upload_images/1419035-cdd0dace733aff10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

![image.png](https://upload-images.jianshu.io/upload_images/1419035-c198455a0c7b4daf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

![image.png](https://upload-images.jianshu.io/upload_images/1419035-35f777c7bdfd168d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)


3.xcode配置

>1.导入sdk `pod 'LinkedME_LinkPage'`
>2.添加依赖库
>>CoreSpotlight.framework**(状态一定要设置为可选)**
>>SystemConfiguration.framework
>>Security.framework
>>WebKit.framework
>>StoreKit.framework

>2.检查BundleID是否和后台一致

>![image.png](https://upload-images.jianshu.io/upload_images/1419035-43ec2c04fddabea0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

>3.查看scheme是否一致
>![image.png](https://upload-images.jianshu.io/upload_images/1419035-4569cb8d9e5df5d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

>4.添加key

![image.png](https://upload-images.jianshu.io/upload_images/1419035-6ea5d968c88eb149.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

>5.检查Assoiciated Domains,是否添加link
>![image.png](https://upload-images.jianshu.io/upload_images/1419035-7900cdae58a7d6a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)


4.将demo运行到真机上

5.扫描二维码能够唤起app代表集成sdk成功

![image.png](https://upload-images.jianshu.io/upload_images/1419035-ef0430854d58e336.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

QA 无法唤起可能出现的原因

>1.参数没有配置正确

>2.清空浏览器缓存

>3.清空xcode缓存

>4.卸载demo

>5.重启手机


通过测试过前端开始 集成js SDk
`key`就是app的linkMe_key,替换到下面即可

完整代码如下

```

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="https://static.lkme.cc/linkedme.min.js" ></script>
    <script>
        linkedme.init("你的key", {type: "live"}, null);
        var data = {};
        // data.type = "live";  //表示现在使用线上模式,如果填写"test", 表示测试模式.【可选】
        // data.feature = "功能名称"; // 自定义深度链接功能，多个用逗号分隔，【可选】
        // data.stage = "阶段名称"; // 自定义深度链接阶段，多个用逗号分隔，【可选】
        // data.channel = "渠道名称"; // 自定义深度链接渠道，多个用逗号分隔，【可选】
        // data.tags = "标签名称"; // 自定义深度链接标签，多个用逗号分隔，【可选】
        // data.ios_custom_url = ""; // 首次集成测试请留空！！！自定义iOS平台下App的下载地址，如果是AppStore的下载地址可以不用填写，需填写http或https【可选】
        // data.ios_direct_open = "false"; //首次集成测试请默认为false！！！true：未安装情况下直接打开ios_custom_url，默认为false【可选】
        // data.android_custom_url = "";// 首次集成测试请留空！！！自定义安卓平台下App的下载地址，需填写http或https【可选】
        // data.android_direct_open = ""; // 首次集成测试请默认为false！！！true:所有情况下跳转android_custom_url，不会走深度链接跳转打开APP的逻辑，默认为false【可选】
        // // 下面是自定义深度链接参数，用户点击深度链接打开app之后，params的参数会通过LinkedME服务器透传给app，由app根据参数进行相关跳转
        // // 例如：详情页面的参数，写入到params中，这样在唤起app并获取参数后app根据参数跳转到详情页面
        // var value1 = 1;
        // var value2 = 2;
        // data.params = '{"key1":"'+value1+'","key2":"'+value2+'"}'; //注意单引号和双引号的位置
        linkedme.link(data, function(err, response){
            if(err){
                // 生成深度链接失败，返回错误对象err
            } else {
                console.log(response);
                // alert(response.url);
                /*
                  生成深度链接成功，深度链接可以通过data.url得到，
                  将深度链接绑定到<a>标签，这样当用户点击这
                  个深度链接，如果是在pc上，那么跳转到深度链接二维
                  码页面，用户用手机扫描该二维码就会打开app；如果
                  在移动端，深度链接直接会根据手机设备类型打开ios
                  或者安卓app
                 */
                document.body.innerHTML += '<a class="linkedme" href="'+response.url+'">LinkedME深度链接</a>'
            }
        },false);
    </script>
</head>
<body>
</body>
</html>

```