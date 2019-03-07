### 关于快应用：
特点：即点即用，原生应用体验。

## 以下是遇到的坑：

### （1）配置信息：
- 支持的最小平台版本号（minPlatformVersion）

支持的最小平台版本号为非必填项，标识开发者的rpk包兼容支持的最小运行平台版本

当使用了1020及以上的平台版本新增特性时，就必须确保minPlatformVersion最低为该平台版本号，避免上线后在更低版本平台上运行出错

示例如下：
```
{
  "minPlatformVersion": 1020
}
```
**踩到的坑：发布审核不通过，当时客服反馈用的是1000版本去测的，因为我这边没有适配1000版本，打开直接就报错了**

### （2）input

|名称   |   类型|  默认值|  必填|  描述| 
| ------|------ | -----|------- |------- |
|   type|   button，checkbox ，radio，text ， email，date，time ，number，password| text| 否| 不支持动态修改| 

**踩到的坑：当时做登录的时候有一个切换显示隐藏密码的功能，这时我修改type的 password和text来做切换，安卓手机不生效，不支持。**


### （3）CSS布局
- 1、CSS3 @keyframes
不支持带top,bottom,left,right属性，只能用translate
支持：
```
@keyframes bottomIn {
    from {
        transform: translateY(1000px)
    }

    to {
        transform: translateY(0)
    }
}
```
不支持：
```
@keyframes bottomIn
{
    from {
        bottom:1000px;
        
    }
    to {
        bottom:20;
        
    }
}
```
- 2、border不支持简写
比如：
```
border: 1px solid #fff;
```

需要写成：
```
border-bottom-color: #dddddd;
border-style: solid;
border-bottom-width: 1px;
```

- 3、不支持阴影属性
```
box-shadow: 10px 10px 5px #888888;
```

### （4）长度单位

(1) 仅支持px和%。

(2) px是相对于项目配置基准宽度的单位，已经适配了移动端屏幕。

(3) 设计稿1px/设计稿基准宽度 = 框架样式1px/ 项目配置基准宽度。

所有当我们拿到视觉稿的时候，视觉稿是多少像素尺寸就用多少。

### （5）OPPO r9m状态栏兼容问题

快应用不支持修改手机显示状态栏颜色，由于OPPO r9m手机状态栏显示是白色的，这时把快应用的titleBarBackgroundColor修改成白色的话，这款手机就不能正常显示状态栏了，如果修改成黑色，其它大部分手机也显示不正常。

### （6）获取授权码code
授权码是用来获取令牌accessToken和跨平台用户标识openId的（做账号关联），开始的时候我试了很多机型都拿不到这个code，返回结果fail，直到我反馈给了快应用客服，这个是需要手机厂商对接你的物联设备才行，各手机厂商对你的快应用包名上传记录。快应用才能拿到code。

### （7）获取clientId和clientSecret
获取accessToken和openId接口不仅需要用到授权code，还需要用到clientId和clientSecret，如果要拿到clientId和clientSecret这些参数，必须把快应用提交审核发布才行，审核发布有2次审核，第一次是快应用的的审核，第二次是个大厂商的审核，提交的应用应该是功能完善的，但是只差账号体系的，只需要通过快应用的审核即可，这时去厂商的开发平台就能找到相关的信息。


### （8）快应用包包体大小在1M以内，否则不能上传
1M的包体积真的很小，没办法，暂时只能按照规矩来，图片要控制好，图片是影响包体积的最大因素。

### （9）正式上传rpk 包签名校验失败
需要通过openssl命令等工具生成签名文件private.pem、certificate.pem，例如：
```
openssl req -newkey rsa:2048 -nodes -keyout private.pem -x509 -days 3650 -out certificate.pem
```
在工程的sign目录下创建release目录，将私钥文件private.pem和证书文件certificate.pem拷贝进去,然后通过 npm run release 来构建正式环境的rpk。


### （10）导航栏

快应用没有现成的导航栏接口提供来控制menu和返回按钮，这时我们可以定制自己的导航栏，
在父组件里，子组件通过$dispatch()，完成返回事件和menu事件的触发。



### （11）快应用不允许提供下载原生应用的功能
页面提供了一个下载原生的途径，提交审核的时候被厂商拒绝了，因为页面存在分发选项，提供下载原生APP应用，这是不允许这样做的。


### 资料查询
- [快应用官网](www.quickapp.cn/)
- [快应用开发文档](doc.quickapp.cn/)
- [快应用论坛](bbs.quickapp.cn/)
- [快应用账号注册流程](www.quickapp.cn/docCenter/post/71)
- [快应用常见问题和技术帖子汇总](https://bbs.quickapp.cn/forum.php?mod=viewthread&tid=838)
- [快应用官方IDE](https://bbs.quickapp.cn/forum.php?mod=viewthread&tid=1052)