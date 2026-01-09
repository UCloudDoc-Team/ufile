# 文档预览

## 概念

文档预览功能支持文字文件、演示文件、表格文件、以及pdf文件的在线预览，便于您进行文档内容的查看。

## 注意事项

- 支持在线预览的文件类型

    - 支持以pdf的格式预览的文件类型：ppt、pptx、wps、doc、docx、xls、xlsx
    - 支持以h5格式预览的文件类型：pptx

- 文件大小限制

    - 不支持在线预览大于200MB的文件

## 调用参数

对于支持在线预览的文件格式，可在文件url上拼接参数 `x-udi-process=doc-preview&dstFormat=${format}` 进行预览

举例说明：

- 对于公共bucket

    `http://{bucketName}.cn-bj.ufileos.com/sample.pptx?x-udi-process=doc-preview&dstFormat=h5`

- 对于私有bucket

    `http://{bucketName}.cn-bj.ufileos.com/sample.pptx?UCloudPublicKey=${publicKey}&Signature=${signature}&Expires=1649832250&x-udi-process=doc-preview&dstFormat=h5`

> dstFormat的值为pdf或h5

## 控制台预览

在文件管理tab页内，对于支持预览的文件，在操作一栏上会有预览按钮，点击即可进行文件预览

![](/images/文件预览1.png)
![](/images/文件预览2.png)

## 开放地域

- 华北（北京2）
- 华北（乌兰察布）
- 上海
- 华南（广州）
