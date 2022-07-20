# cmy-unit-open-docs

### 介绍
财猫云智能咨询 WebService API 是基于HTTPS/HTTP协议的数据接口，开发者可以使用任何客户端、服务器和开发语言，按照财猫云智能咨询 WebService API 规范，按需构建HTTPS请求，并获取结果数据（目前支持JSON/JSONP方式返回）。

### 使用限制
目前针对企业开发者，提供的服务调用量有差别，可参考配额限制说明。


### 开始接入

1.  申请开发者密钥（Key）：申请密钥，请联系我们的客服人员。
2.  本示例的Key仅为演示使用，实际开发及上线时，请务必使用您申请的Key。以下示例为：如何申请发票？。


```js
https://ai.wencaishui.com/unit/uskit/bot/chat?key=4A6F70D4-54C9-440C-823A-01FB1BC33C54
```
- Curl

```bash
curl -X 'POST' \
  'https://ai.wencaishui.com/unit/uskit/bot/chat?key=4A6F70D4-54C9-440C-823A-01FB1BC33C54' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "userId": "test001",
  "sessionId": "",
  "query": "如何申请发票？",
  "chatType": "cc"
}'
```

```bash
{
    "errcode": 0,
    "errmsg": "ok",
    "data": {
        "option_list": [],
        "type": "clarify",
        "say": "请问你是要开普通发票还是专用发票？",
        "session_id": "session-1658291343219-3095040582-8013-6321"
    }
}
```
- Postman

![2022-07-20_13-01](https://user-images.githubusercontent.com/75599950/179907232-d1962ac8-ca4f-4364-b2f7-13c64fce165f.png)

### 参数说明

- 输入参数

| 名称  | 类型  | 是否必须 | 说明  |
|---|---|---|---|
| user_id  | string  | Y  | 对话的用户id，用来区分唯一标识  |
| session_id  | string  | Y  | 会话id。详情见【请求参数详细说明】 |
| query  | string  | Y  | 咨询的内容  |
| chat_type  | string  | Y  | 对话类型(wechat：微信  web：网站  cc：呼叫中心)  |

请求参数详细说明

1. session_id

session保存机器人的历史会话信息，由机器人创建，客户端从上轮应答中取出并直接传递，不需要了解其内容。如果为空，则表示清空session。

2. chat_type

不同的对话类型，在返回处理上会有所不同。

- 输出参数

|  名称 | 类型  | 是否必备  | 说明  |
|---|---|---|---|
| errcode  |  int |  Y | 错误码，为0时表示成功  |
| errmsg  |  string |  N | 错误信息，error_code!= 0 时存在  |
|  data |  jsonObject |  N |  结果信息，error_code= 0 时存在 |

data结构：

|  名称 | 类型  | 是否必备  | 说明  |
|---|---|---|---|
|  type |  string | Y  |  动作类型。详情见【输出参数详细说明】 |
|  say |  string | Y  |  应答话术 |
|  session_id |  string | Y  |  会话ID |
|  option_list |  jsonArray | N  |  选项列表 |

option_list结构：

|  名称 | 类型  | 是否必备  | 说明  |
|---|---|---|---|
|  id |  int | Y  |  候选值ID |
|  option |  string | Y  |  候选值 |

输出参数详细说明

1.type

动作类型，具体有以下几种:
| 类型  |  说明 |
|---|---|
| satisfy  | 满足: 语义被直接理解并直接给出了回答 |
| clarify  | 澄清: 表示需要提问者补充澄清某些变量，比如纳税人类型、税种等 |
| guide  | 引导: 表示有多个可能的候选问题，或者需进一步引导到其他对话意图 |
| failure  | 语音理解失败  |


### 服务安全

财猫云智能咨询服务API Key，在调用时用于唯一标识开发者身份。一般用于开发环境。生产环境，请使用 API 签名认证调用方法（AppKey & AppSecret），具体请联系客服人员。

### 状态码说明

- HTTP状态码

以下状态码来自HTTP响应头（Response Headers）中的status，代表接入层服务运行状态，一情况下为200，
财猫云智能咨询WebService在HTTP响应body中（JSON）定义了更为清晰的状态码，请参考下文

| 状态码  | 说明  |
|---|---|
| 200  |  OK：请求成功 |
|  401 |  Unauthorized：未经授权 |
|  403 |  Forbidden：鉴权没有通过，没有权限访问资源 |

- 响应结果(JSON)中的status状态码

当http状态码为200时，服务会返回响应结果（JSON格式），在JSON对象中的status参数，代表服务的最终响应状态码，其含义如下：

| 错误码  | 错误信息  |  描述 |
|---|---|---|
| -1  | 系统繁忙，请稍后再试  |  如果持续出现该错误，请联系技术团队 |
|  1001 | 非法请求参数  | 请求参数格式不正确  |
|  1002 |  请求IP没有被授权 |  请提供服务器IP，联系技术团队配置IP白名单 |
|  1003 |  服务内部错误 |  请联系技术团队 |
