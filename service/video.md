# 用超分转码提升 AIGC 视频画质

本文介绍如何用 UFile 的视频超分转码接口（`CreateUFileMediaTask` / `DescribeUFileMediaTask`），把 AIGC 模型产出的低分辨率视频放大修复为 1080P、2K 或 4K，并用画质评测接口量化超分前后的效果。

## 1. 适用场景

AIGC 视频生成模型直接出 1080P 或 4K，算力开销和生成耗时都远高于 720P，且长视频容易中途失败。更划算的做法是：**让模型只出 720P 等低分辨率成片，再用超分转码放大到目标分辨率**。放大与画质修复在同一条转码管线内完成，不需要额外步骤。

典型场景：

- **AIGC 视频降本增效**：生成环节出 720P，交付环节超分到 1080P/4K。
- **AIGC 成片画质打磨**：修复模型产物常见的边缘模糊、纹理不足、轻微压缩失真。
- **老片高清重制**：低分辨率存量素材重制为高清版本。
- **低质量片源修复**：UGC、翻录、多次转存后画质受损的视频。


## 2. 前提条件

- 已开通 UFile 对象存储，并已开通数据智能服务（超分转码与画质评测依赖该服务）。

![image](/images/pic/video1.png)

- 源视频已上传到 UFile 存储空间；输出路径也在 UFile 存储空间内。
- **源存储空间与输出存储空间必须在同一地域**。任务记录按地域存储，地域由源存储空间自动确定。
- 调用账号对源存储空间和输出存储空间都有访问权限，且两者归属当前公司与项目。
- 目前只支持华北(乌兰察布)地域。


## 3. 选择转码模板

转码模板由 `Template` 参数指定，当前可用的模板共 4 个，均属于转码服务商 `vis`（微帧）：

| Template | 输出分辨率 |
|---|---|
| `VIS-720P-UMedia` | 720P |
| `VIS-1080P-UMedia` | 1080P |
| `VIS-2K-UMedia` | 2K |
| `VIS-4K-UMedia` | 4K |

`Vendor`（转码服务商）参数可以不传，默认即为 `vis`。

按 AIGC 成片的源分辨率选目标档位：

| 源分辨率 | 推荐 Template |
|---|---|
| 480P / 540P | `VIS-720P-UMedia` |
| 720P | `VIS-1080P-UMedia` |
| 1080P | `VIS-2K-UMedia` / `VIS-4K-UMedia` |

模板按**目标分辨率**分档，输出分辨率以模板档位为准，不是按固定倍率无限放大。目标分辨率高于源视频分辨率时执行超分处理；填低于源分辨率的档位则是降分辨率转码，不会触发超分。

## 4. 调用方式

接口为 POST，请求体为 JSON，通过 `Action` 字段指定接口名：

```
POST https://api.ucloud.cn/
Content-Type: application/json
```

请求需携带 UCloud OpenAPI 公共参数（`Action`、`PublicKey`、`Signature` 等），签名算法详见 UCloud 开放平台《API 调用方式》文档，本文只列业务参数。

公共返回字段：

| 字段 | 类型 | 说明 |
|---|---|---|
| `Action` | string | 固定为 `<请求 Action>Response` |
| `RetCode` | int | `0` 表示成功，非 0 见第 9 节「错误码」 |
| `Message` | string | 仅失败时返回，错误描述 |

## 5. 步骤一：提交超分任务

`Action` 为 `CreateUFileMediaTask`。

### 请求参数

| 参数 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|
| `TaskName` | string | 是 | — | 任务名称，便于识别，不要求唯一 |
| `BucketName` | string | 是 | — | 源视频所在存储空间 |
| `SourceObject` | string | 是 | — | 源视频完整路径，如 `aigc/720p-draft.mp4` |
| `OutputObject` | string | 是 | — | 输出视频完整路径，如 `aigc/1080p-final.mp4` |
| `OutputBucket` | string | 否 | 同 `BucketName` | 输出存储空间，须与源存储空间同地域 |
| `Template` | string | 是 | — | 转码模板，取值见第 3 节 |
| `ProjectId` | string | 否 | — | 项目标识 |

任务所属地域由 `BucketName` 自动确定，无需指定。

### 返回字段

| 字段 | 类型 | 说明 |
|---|---|---|
| `TaskId` | string | 任务 ID，查询任务状态时使用 |

### 请求示例

720P AIGC 成片超分到 1080P：

```json
{
    "Action": "CreateUFileMediaTask",
    "TaskName": "aigc-720p-to-1080p",
    "BucketName": "my-bucket",
    "SourceObject": "aigc/720p-draft.mp4",
    "OutputBucket": "my-bucket",
    "OutputObject": "aigc/1080p-final.mp4",
    "Vendor": "vis",
    "Template": "VIS-1080P-UMedia",
    "ProjectId": "org-xxxxxx"
}
```

### 返回示例

```json
{
    "Action": "CreateUFileMediaTaskResponse",
    "RetCode": 0,
    "TaskId": "d935de87-e591-4b89-a1e4-2544a36457e4"
}
```

### 同时产出多个分辨率

接口一次只接受一个输出。要同时产出 1080P 和 4K，提交两个任务，各自指定不同的 `Template` 与 `OutputObject`：

```json
{
    "Action": "CreateUFileMediaTask",
    "TaskName": "aigc-720p-to-4k",
    "BucketName": "my-bucket",
    "SourceObject": "aigc/720p-draft.mp4",
    "OutputObject": "aigc/4k-final.mp4",
    "Vendor": "vis",
    "Template": "VIS-4K-UMedia"
}
```

## 6. 步骤二：查询任务状态

`Action` 为 `DescribeUFileMediaTask`。传 `TaskId` 查单个任务，不传则返回任务列表。

### 请求参数

| 参数 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|
| `Region` | string | 是 | — | 任务所在地域，与源存储空间地域一致，如 `cn-wlcb` |
| `TaskId` | string | 否 | — | 任务 ID，不传时返回任务列表 |
| `BucketName` | string | 否 | — | 按源存储空间过滤 |
| `Vendor` | string | 否 | — | 按转码服务商过滤 |
| `Status` | string | 否 | — | 按任务状态过滤，取值见下 |
| `Offset` | int | 否 | `0` | 分页偏移量 |
| `Limit` | int | 否 | `20` | 单页条数，1–1000 |

任务按地域存储，查询必须指定 `Region`，不支持跨地域查询。

### 任务状态

| Status | 说明 |
|---|---|
| `init` | 任务已创建，等待调度 |
| `running` | 转码进行中 |
| `success` | 转码成功，输出文件已生成（终态） |
| `fail` | 转码失败（终态） |

失败态取值是 `fail`，**不是 `failed`**。按 `failed` 过滤或比对会一条都匹配不到，而且「查不到」与「确实没有失败任务」无法区分。

### 返回字段

| 字段 | 类型 | 说明 |
|---|---|---|
| `TotalCount` | int | 符合条件的任务总数，不是当前页条数 |
| `DataSet` | array | 任务列表，按创建时间倒序 |

`DataSet` 元素：`TaskId`、`TaskName`、`BucketName`、`SourceObject`、`OutputBucket`、`OutputObject`、`Vendor`、`Template`、`Status`、`CTime`、`MTime`、`CompletedTime`。三个时间字段均为 Unix 秒级时间戳，`CompletedTime` 未完成时为 `0`。

### 请求示例

```json
{
    "Action": "DescribeUFileMediaTask",
    "Region": "cn-wlcb",
    "TaskId": "d935de87-e591-4b89-a1e4-2544a36457e4"
}
```

### 返回示例

```json
{
    "Action": "DescribeUFileMediaTaskResponse",
    "RetCode": 0,
    "TotalCount": 1,
    "DataSet": [
        {
            "TaskId": "d935de87-e591-4b89-a1e4-2544a36457e4",
            "TaskName": "aigc-720p-to-1080p",
            "BucketName": "my-bucket",
            "SourceObject": "aigc/720p-draft.mp4",
            "OutputBucket": "my-bucket",
            "OutputObject": "aigc/1080p-final.mp4",
            "Vendor": "vis",
            "Template": "VIS-1080P-UMedia",
            "Status": "success",
            "CTime": 1792468090,
            "MTime": 1792468274,
            "CompletedTime": 1792468274
        }
    ]
}
```

### 轮询建议

- **间隔不低于 1 分钟**。任务提交后要等调度，状态不会立即变化，高频查询不会加快处理。
- **必须处理 `fail`**。只等 `success` 会在任务失败时陷入无限等待。
- 超分比普通转码多一层 AI 画质重建，计算量更大，**4K 尤其耗时**；耗时主要取决于源视频的分辨率与时长。短视频通常数分钟内完成，长视频请预留更多时间。

## 7. 步骤三（可选）：用画质评测验证超分效果

超分效果是否达标可以用画质评测接口量化：**对源视频和超分成片各跑一次评测，对比 `Score`**。这是批量处理前验证模板选择的推荐做法。

### 提交评测任务

`Action` 为 `CreateUFileMediaEvalTask`。

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `TaskName` | string | 是 | 评测任务名称 |
| `BucketName` | string | 是 | 待评测视频所在存储空间 |
| `SourceObject` | string | 是 | 待评测视频完整路径 |
| `ProjectId` | string | 否 | 项目标识 |

返回 `TaskId`。评测接口只接受单个视频，不做两个文件的对比，超分前后的对比需要自己提交两个任务。

```json
{
    "Action": "CreateUFileMediaEvalTask",
    "TaskName": "eval-1080p-final",
    "BucketName": "my-bucket",
    "SourceObject": "aigc/1080p-final.mp4"
}
```

### 查询评测结果

`Action` 为 `DescribeUFileMediaEvalTask`，请求参数与 `DescribeUFileMediaTask` 一致（`Region` 必填，支持 `TaskId`、`BucketName`、`Status`、`Offset`、`Limit`）

`DataSet` 元素在任务字段之外多出 `QualityScore`，两者都在评测完成后才有值，未完成时字段省略。

`QualityScore` 各维度：

| 字段 | 取值 | 说明 |
|---|---|---|
| `Score` | 0–5 | 整体质量分，**越大画质越好** |
| `Block` | 0–1 | 压缩噪声与块效应强度，越小越好 |
| `Blur` | 0–1 | 模糊程度，越小越好 |
| `Noise` | 0–1 | 噪声强度，越小越好 |
| `Aesthetics` | 0–1 | 美学评分，越大越好 |
| `Color` | 0–1 | 色彩丰富程度 |
| `LowContrast` | 0–1 | 对比度偏低概率，越小越好 |
| `OverSharpen` | 0–1 | 过度锐化概率，越小越好 |
| `Sandwich` | 0–1 | 三明治效应概率，越小越好 |

返回示例：

```json
{
    "Action": "DescribeUFileMediaEvalTaskResponse",
    "RetCode": 0,
    "TotalCount": 1,
    "DataSet": [
        {
            "TaskId": "45ddfd24-3a79-4738-a589-7e8492e82c11",
            "TaskName": "eval-1080p-final",
            "BucketName": "my-bucket",
            "SourceObject": "aigc/1080p-final.mp4",
            "Status": "success",
            "QualityScore": {
                "Score": 4.12,
                "Block": 0.08,
                "Blur": 0.11,
                "Noise": 0.07,
                "Aesthetics": 0.63,
                "Color": 0.58,
                "LowContrast": 0.04,
                "OverSharpen": 0.09,
                "Sandwich": 0.02
            },
            "CTime": 1792468300,
            "MTime": 1792468420,
            "CompletedTime": 1792468420
        }
    ]
}
```

### 判读建议

AIGC 成片超分后，`Score` 应上升，`Blur` 明显下降。如果 `OverSharpen` 显著升高，说明该档位对这类片源锐化过度，可以降一档目标分辨率再试。

## 8. 使用限制与注意事项

- **源文件必须是视频**，按文件后缀校验，后缀不区分大小写。支持 `.mp4` `.m4v` `.mov` `.mkv` `.webm` `.avi` `.wmv` `.asf` `.flv` `.f4v` `.ts` `.mts` `.m2ts` `.mpg` `.mpeg` `.m2v` `.3gp` `.3g2` `.ogv` `.rm` `.rmvb` `.vob`。无后缀或后缀不在列表内直接报错；真正的容器与编码校验在转码侧完成。
- **接口不校验源视频是否存在**。源视频不存在时任务仍会创建成功，错误在转码阶段才返回。
- **输出路径不能与源路径相同**（同一存储空间内），提交即报错。
- **输出会覆盖输出路径上的同名文件**，请确认该路径可写且不含需要保留的文件。
- 源存储空间与输出存储空间必须同地域；跨地域会报参数非法。
- 输出文件体积通常显著大于源文件，分辨率越高增幅越大，请预留存储空间。
- 任务创建后无法取消或修改参数，需要调整请重新创建。
- 任务名称可重复，请以 `TaskId` 作为唯一标识。
- 查询接口只返回当前公司与项目下的任务。
- 超分转码单价高于普通转码，输出分辨率越高越贵，另有 UFile 存储与流量费用。**批量处理前建议先用一小段片源验证画质与耗时。**

## 9. 错误码

| RetCode | 说明 | 处理建议 |
|---|---|---|
| 0 | 成功 | — |
| 230 | 参数值不可用 | 检查必填参数是否传了空值 |
| 15002 | 缺少必填参数 | 按参数表补齐 |
| 15004 | 服务内部错误 | 稍后重试，持续出现请联系技术支持 |
| 15010 | 存储空间不存在或无访问权限 | 检查空间名称，确认归属当前公司与项目 |
| 15017 | 参数非法 | 检查 `Region` 取值；确认输出路径未与源路径重复；确认输出空间与源空间同地域 |
| 15018 | 参数超出取值范围 | 检查 `Limit` 是否在 1–1000 之间 |
| 15031 | 渠道不支持 | 检查 `channel` 取值 |
| 15117 | 转码模板不支持 | 确认 `Template` 为第 3 节列出的 4 个模板之一 |
| 15118 | 转码任务不存在 | 检查 `TaskId`，确认任务属于当前公司与项目 |
| 15119 | 转码服务商不支持 | `Vendor` 仅支持 `vis`，建议不传该参数 |
| 15120 | 评测任务不存在 | 检查评测 `TaskId` |
| 15121 | 源文件不是支持的视频格式 | 检查 `SourceObject` 后缀，见第 9 节 |

## 10. 常见问题

**超分能把任意视频放大到 4K 吗？**
可以提交，但输出分辨率固定为模板档位，不按倍率无限放大。源分辨率过低（如 360P）直接拉到 4K，画质提升有限且耗时与费用更高，建议逐档验证。

**为什么查不到任务？**
最常见的三个原因：`Region` 与源存储空间地域不一致、按 `failed` 而不是 `fail` 过滤状态、任务不属于当前公司与项目。

