

# 创建存储空间

在空间管理页点击左上角操作栏的“创建存储空间”。

![](/images/创建存储空间1.png)

在“创建存储空间”弹窗页中，可以自定义存储地域、空间类型、业务组、存储空间域名，选择是否授权开启数据分析。

![](/images/创建存储空间2.png)

|字段 |说明 |
|---- |---- |
|选择地域 |您可就近选择一个地域，作为存储空间中文件对象的物理存储位置，存储空间创建后无法更换地域。如需要通过 UHost 云主机内网访问 US3 对象存储，需要选择与您 UHost 节点相同的地域。 |
|空间类型 |公开空间：所有文件可通过URL直接访问。 <br> 私有空间：所有文件须获得拥有者的 API 密钥授权才能访问。|
|业务组 |您可选择或创建一个业务组，将存储空间按业务组进行归类，便于查询。 |
|存储空间域名 |填写存储空间对应的域名地址作为对外的访问地址，由于存储空间域名全局唯一，遇到冲突请更换名称。 |
|数据分析 |选择是否授权 USQL 进行数据湖分析，开启后，您可通过 USQL 数据分析产品对空间内文本数据进行挖掘与分析。 |

确定后即创建存储空间成功。存储空间创建成功后，用户在控制台 SDK 及工具页下载客户端管理工具进行空间和文件管理操作，同时也可以使用 API 或 SDK 进行空间和文件管理操作。
