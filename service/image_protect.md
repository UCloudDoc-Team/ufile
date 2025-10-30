# 原图保护

原图保护功能可以防止US3内允许匿名访问的图片文件被盗用，开启原图保护后，匿名者可以使用携带样式参数的请求或通过签名URL访问原图,目前仅支持新加坡、伦敦、首尔、香港、拉各斯、胡志明、迪拜以及台北地域，其他地域若有需要，请联系技术支持。

## 访问方式
用户可以通过以下方式访问开启原图保护的图片：
- 使用携带样式参数的文件URL访问，格式为:https://bucketname.endpoint/objectname?iopstyle=stylename
- 使用携带签名的文件URL访问，格式为:https://bucketname.endpoint/objectname?signature


## 操作步骤

1. 登陆US3管理控制台
![image](/images/pic/us3.png)
2. 在单地域空间管理，单击目标Bucket
![image](/images/pic/bucket.png)
3. 选择数据处理>图片处理，然后单击访问设置。
![image](/images/pic/image_set.png)
4. 在访问设置面板，打开原图保护开关，单击确定保存
![image](/images/pic/image_protect.png)
- 原图保护后缀：在原图保护后缀下拉列表选择文件后缀，Bucket内所有对应后缀的图片文件都会被保护,（选择 * 时，原图保护功能将应用到桶内所有对象，包含图片及非图片类型对象。）。

## 注意事项

- 原图保护仅针对允许匿名访问的文件，使用签名URL访问图片属于非匿名访问。所以开启原图保护后，如果使用带签名的URL、或者CDN回源到了私有Bucket，仍然可以访问原图。
