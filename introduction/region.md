

# 地域和域名

US3 对象存储提供外网、内网域名两种访问方式：外网域名可响应所有来自于互联网的访问请求，内网域名可响应来自于同一地域下的 UCloud 公有云服务的访问请求（如：UHost 云主机、UK8S 容器云等）。

***若使用外网域名对数据进行访问，会产生外网流出流量费用。***

在您创建存储空间后，您存储空间的访问域名地址格式为：`<存储空间名称>.<域名地址>`。

**公有云下US3协议各地域访问域名（Endpoint）如下：**

| 地域 | 外网域名 | 内网域名 |
| ---- | -------- | -------- |
| 华北（北京2）  | cn-bj.ufileos.com      | ufile.cn-north-02.ucloud.cn      |
| 华北（乌兰察布）  | cn-wlcb.ufileos.com      | internal-cn-wlcb.ufileos.com      |
| 华东（上海2） | cn-sh2.ufileos.com      | internal-cn-sh2-01.ufileos.com      |
| 华南（广州）  | cn-gd.ufileos.com      | internal-cn-gd-02.ufileos.com      |
| 香港  | hk.ufileos.com      | internal-hk-01.ufileos.com     |
| 美国（洛杉矶） | us-ca.ufileos.com      | internal-us-ca-01.ufileos.com     |
| 新加坡 | sg.ufileos.com      | internal-sg-01.ufileos.com     |
| 印度尼西亚（雅加达） | idn-jakarta.ufileos.com      | internal-idn-jakarta-01.ufileos.com     |
| 台湾（台北）  | tw-tp.ufileos.com      | internal-tw-tp.ufileos.com     |
| 尼日利亚（拉各斯） | afr-nigeria.ufileos.com      | internal-afr-nigeria.ufileos.com     |
| 巴西（圣保罗） | bra-saopaulo.ufileos.com      | internal-bra-saopaulo.ufileos.com     |
| 阿联酋（迪拜）  | uae-dubai.ufileos.com      | internal-uae-dubai.ufileos.com     |
| 德国（法兰克福）  | ge-fra.ufileos.com     | internal-ge-fra.ufileos.com     |
| 越南（胡志明）  | vn-sng.ufileos.com      | internal-vn-sng.ufileos.com     |
| 美国（华盛顿）  | us-ws.ufileos.com      | internal-us-ws.ufileos.com     |
| 印度（孟买）  | ind-mumbai.ufileos.com      | internal-ind-mumbai.ufileos.com     |
| 韩国（首尔）  | kr-seoul.ufileos.com      | internal-kr-seoul.ufileos.com     |
| 日本（东京）  | jpn-tky.ufileos.com      | internal-jpn-tky.ufileos.com     |
| 泰国（曼谷）  | th-bkk.ufileos.com      | internal-th-bkk.ufileos.com     |
| 英国	| uk-london.ufileos.com | internal-uk-london.ufileos.com |
| 俄罗斯（莫斯科） | rus-mosc.ufileos.com | internal-rus-mosc.ufileos.com |
| 贵阳一 | cn-guiyang1.ufileos.com | internal-cn-guiyang1.ufileos.com |

**公有云下AWS S3协议各地域访问域名（Endpoint）如下：**

| **编号** | **地域** | **外网Endpoint**            | **内网Endpoint**                     |
| :------- | :------- | :-------------------------- | :----------------------------------- |
| 1        | 华北（北京2）   | s3-cn-bj.ufileos.com        | internal.s3-cn-bj.ufileos.com        |
| 2        | 华北（乌兰察布）   | s3-cn-wlcb.ufileos.com      | internal.s3-cn-wlcb.ufileos.com      |
| 3        | 上海     | s3-cn-sh2.ufileos.com       | internal.s3-cn-sh2.ufileos.com       |
| 4        | 华南（广州）     | s3-cn-gd.ufileos.com        | internal.s3-cn-gd.ufileos.com        |
| 5        | 香港     | s3-hk.ufileos.com           | internal.s3-hk.ufileos.com           |
| 6        | 美国（洛杉矶）   | s3-us-ca.ufileos.com        | internal.s3-us-ca.ufileos.com        |
| 7        | 新加坡   | s3-sg.ufileos.com           | internal.s3-sg.ufileos.com           |
| 8        | 印度尼西亚（雅加达）   | s3-idn-jakarta.ufileos.com  | internal.s3-idn-jakarta.ufileos.com  |
| 9        | 台湾（台北）     | s3-tw-tp.ufileos.com        | internal.s3-tw-tp.ufileos.com        |
| 10       | 尼日利亚（拉各斯）   | s3-afr-nigeria.ufileos.com  | internal.s3-afr-nigeria.ufileos.com  |
| 11       | 巴西（圣保罗）   | s3-bra-saopaulo.ufileos.com | internal.s3-bra-saopaulo.ufileos.com |
| 12       | 阿联酋（迪拜）     | s3-uae-dubai.ufileos.com    | internal.s3-uae-dubai.ufileos.com    |
| 13       | 德国（法兰克福） | s3-ge-fra.ufileos.com       | internal.s3-ge-fra.ufileos.com       |
| 14       | 越南（胡志明） | s3-vn-sng.ufileos.com       | internal.s3-vn-sng.ufileos.com       |
| 15       | 美国（华盛顿）   | s3-us-ws.ufileos.com        | internal.s3-us-ws.ufileos.com        |
| 16       | 印度（孟买）     | s3-ind-mumbai.ufileos.com   | internal.s3-ind-mumbai.ufileos.com   |
| 17       | 韩国（首尔）     | s3-kr-seoul.ufileos.com     | internal.s3-kr-seoul.ufileos.com     |
| 18       | 日本（东京）     | s3-jpn-tky.ufileos.com      | internal.s3-jpn-tky.ufileos.com      |
| 19       | 泰国（曼谷）     | s3-th-bkk.ufileos.com       | internal.s3-th-bkk.ufileos.com       |
| 20       | 英国（伦敦）     | s3-uk-london.ufileos.com    | internal.s3-uk-london.ufileos.com    |
| 21       | 俄罗斯（莫斯科）   | s3-rus-mosc.ufileos.com     | internal.s3-rus-mosc.ufileos.com     |
| 22       | 贵阳一   | s3-cn-guiyang1.ufileos.com  | internal.s3-cn-guiyang1.ufileos.com  |

**注意：**

**1. 由于存储集群所在地域不同，跨国访问可能会存在响应延迟或失败的情况。** 

**2. 兼容支持AWS S3协议的访问域名请参见[AWS S3协议支持说明](/ufile/s3/s3_introduction)。**

**3. US3默认外网域名， 下载场景会存在速度，文件类型的限制； 外网下载场景建议使用自定义域名**


