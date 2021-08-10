## 功能描述

CreateWorkflow 用于新增工作流。

## 请求

### 请求实例

```shell
POST /workflow?projectId=xxx HTTP/1.1
Host: iss.<Region>.myqcloud.com
Date: <GMT Date>
Authorization: <Auth String>
Content-Length: <length>
Content-Type: application/xml

<body>
```

> Authorization: Auth String （详情请查阅 [请求签名](https://cloud.tencent.com/document/product/436/7778) 文档）。

### 请求头

#### 公共头部

该请求操作的实现使用公共请求头，了解公共请求头详情请参阅 [公共请求头部](https://cloud.tencent.com/document/product/460/42865) 文档。

#### 非公共头部

该请求操作无特殊的请求头部信息。

#### 请求参数

参数的具体内容如下：

| 节点名称（关键字） | 描述   | 类型   | 必选 |
| :----------------- | :----- | :----- | :--- |
| projectId          | 项目Id | String | 是   |

<br>


### 请求体

该请求操作的实现需要有如下请求体。

```shell
<Request>
    <IssWorkflow>
        <Name>demo</Name>
        <NotifyConfig>
                <Url>http://notify.demo.com</Url>
                <Event>WorkflowFinish</Event>
                <State>On</State>
        </NotifyConfig>
        <Topology>
            <Dependencies>
                <Start>AI_1581665960536,AI_1581665960537/Start>
                <AI_1581665960536>End</AI_1581665960536>
                <AI_1581665960537>End</AI_1581665960537>
            </Dependencies>
            <Nodes>
                <Start>
                    <Type>Start</Type>
                </Start>
                <AI_1581665960536>
                    <Type>AI</Type>
                    <Operation>
                        <TemplateId>t1460606b9752148c4ab182f55163ba7cd</TemplateId>
                    </Operation>
                </AI_1581665960536>
                <AI_1581665960537>
                    <Type>AI</Type>
                    <Operation>
                        <TemplateId>t1460606b9752148c4ab182f55163ba7cd</TemplateId>
                    </Operation>
                </AI_1581665960537>
            </Nodes>
        </Topology>
    </IssWorkflow>
</Request>

```

具体数据描述如下：

| 节点名称（关键字） | 父节点 | 描述           | 类型      | 必选 |
| ------------------ | ------ | -------------- | --------- | ---- |
| Request            | 无     | 保存请求的容器 | Container | 是   |

Container 类型 Request 的具体数据描述如下：

| 节点名称（关键字） | 父节点  | 描述       | 类型      | 必选 |
| ------------------ | ------- | ---------- | --------- | ---- |
| IssWorkflow        | Request | 工作流节点 | Container | 是   |


Container 类型 IssWorkflow 的具体数据描述如下：

| 节点名称（关键字） | 父节点              | 描述       | 类型      | 必选 | 限制                                        |
| ------------------ | ------------------- | ---------- | --------- | ---- | ------------------------------------------- |
| Name               | Request.IssWorkflow | 工作流名称 | String    | 是   | 支持中文、英文、数字、—和_，长度限制128字符 |
| State              | Request.IssWorkflow | 工作流状态 | String    | 否   | On/Off，默认是Off                           |
| NotifyConfig       | Request.IssWorkflow | 回调配置   | Container | 否   | 无                                          |
| Topology           | Request.IssWorkflow | 拓扑信息   | Container | 是   | 无                                          |

Container节点NotifyConfig的内容：

| 节点名称（关键字） | 父节点                           | 描述                                          | 类型   |
| :----------------- | :------------------------------- | :-------------------------------------------- | :----- |
| Url                | Request.IssWorkflow.NotifyConfig | 回调地址                                      | String |
| State              | Request.IssWorkflow.NotifyConfig | 开关状态，On 或者 Off                         | String |
| Event              | Request.IssWorkflow.NotifyConfig | 任务完成:TaskFinish;工作流完成:WorkflowFinish | String |

Container 类型 Topology 的具体数据描述如下：

| 节点名称（关键字） | 父节点                       | 描述         | 类型      | 必选 | 限制 |
| ------------------ | ---------------------------- | ------------ | --------- | ---- | ---- |
| Dependencies       | Request.IssWorkflow.Topology | 节点依赖关系 | Container | 是   | 无   |
| Nodes              | Request.IssWorkflow.Topology | 节点列表     | Container | 是   | 无   |

Container 类型 Nodes 的具体数据描述如下：

| 节点名称（关键字） | 父节点                             | 描述       | 类型      | 必选 | 限制                                  |
| ------------------ | ---------------------------------- | ---------- | --------- | ---- | ------------------------------------- |
| Start              | Request.IssWorkflow.Topology.Nodes | 开始节点   | Container | 是   | 只有唯一一个开始节点                  |
| AI_***             | Request.IssWorkflow.Topology.Nodes | AI任务节点 | Container | 是   | 节点名称以AI_为前缀，可能有多个AI节点 |

Container 类型 Start 的具体数据描述如下：

| 节点名称（关键字） | 父节点                                   | 描述     | 类型   | 必选 | 限制  |
| ------------------ | ---------------------------------------- | -------- | ------ | ---- | ----- |
| Type               | Request.IssWorkflow.Topology.Nodes.Start | 节点类型 | String | 是   | Start |

Container 类型 AI_*** 的具体数据描述如下：

| 节点名称（关键字） | 父节点                                    | 描述     | 类型      | 必选 | 限制 |
| ------------------ | ----------------------------------------- | -------- | --------- | ---- | ---- |
| Type               | Request.IssWorkflow.Topology.Nodes.AI_*** | 节点类型 | String    | 是   | AI   |
| Operation          | Request.IssWorkflow.Topology.Nodes.AI_*** | 操作规则 | Container | 是   | 无   |

Container 类型 Operation 的具体数据描述如下：

| 节点名称（关键字） | 父节点                                             | 描述   | 类型   | 必选 | 限制 |
| ------------------ | -------------------------------------------------- | ------ | ------ | ---- | ---- |
| TemplateId         | Request.IssWorkflow.Topology.Nodes.AI***.Operation | 模板ID | String | 是   | 无   |

## 响应

### 响应头

#### 公共响应头

该响应包含公共响应头，了解公共响应头详情请参阅 [公共响应头部]( https://cloud.tencent.com/document/product/460/42866) 文档。

#### 特有响应头

该响应无特殊的响应头。

### 响应体

该响应体返回为 **application/xml** 数据，包含完整节点数据的内容展示如下：

``` shell
<Response>
    <IssWorkflow>
        <Name>demo</Name>
        <ProjectId></ProjectId>
        <WorkflowId></WorkflowId>
        <State></State>
        <NotifyConfig>
                <Url>http://notify.demo.com</Url>
                <Event>WorkflowFinish</Event>
                <State>On</State>
        </NotifyConfig>
        <Topology>
            <Dependencies>
                <Start>AI_1581665960536,AI_1581665960537/Start>
                <AI_1581665960536>End</AI_1581665960536>
                <AI_1581665960537>End</AI_1581665960537>
            </Dependencies>
            <Nodes>
                <Start>
                    <Type>Start</Type>
                </Start>
                <AI_1581665960536>
                    <Type>AI</Type>
                    <Operation>
                        <TemplateId>t1460606b9752148c4ab182f55163ba7cd</TemplateId>
                    </Operation>
                </AI_1581665960536>
                <AI_1581665960537>
                    <Type>AI</Type>
                    <Operation>
                        <TemplateId>t1460606b9752148c4ab182f55163ba7cd</TemplateId>
                    </Operation>
                </AI_1581665960537>
            </Nodes>
        </Topology>
        <CreateTime></CreateTime>
        <UpdateTime></UpdateTime>
    </IssWorkflow>
</Response>

```

具体的数据内容如下：

| 节点名称（关键字） | 父节点 | 描述                                                         | 类型      |
| :----------------- | :----- | :----------------------------------------------------------- | :-------- |
| Response           | 无     | 保存结果的容器，同Describe Workflow中的Response.IssWorkflowList | Container |


### 错误码

常见的错误信息请参阅 [错误码](https://cloud.tencent.com/document/product/460/42867) 文档。

## 实际案例

### 请求

```shell
POST /workflow HTTP/1.1
Authorization:q-sign-algorithm=sha1&q-ak=AKIDZfbOAo7cllgPvF9cXFrJD0**********&q-sign-time=1497530202;1497610202&q-key-time=1497530202;1497610202&q-header-list=&q-url-param-list=&q-signature=28e9a4986df11bed0255e97ff90500557e0ea057
Host: iss.ap-beijing.myqcloud.com
Content-Length: 166
Content-Type: application/xml

<Request>
    <IssWorkflow>
        <Name>demo</Name>
        <NotifyConfig>
                <Url>http://notify.demo.com</Url>
                <Event>WorkflowFinish</Event>
                <State>On</State>
        </NotifyConfig>
        <Topology>
            <Dependencies>
                <Start>AI_1581665960536,AI_1581665960537/Start>
                <AI_1581665960536>End</AI_1581665960536>
                <AI_1581665960537>End</AI_1581665960537>
            </Dependencies>
            <Nodes>
                <Start>
                    <Type>Start</Type>
                </Start>
                <AI_1581665960536>
                    <Type>AI</Type>
                    <Operation>
                        <TemplateId>t1460606b9752148c4ab182f55163ba7cd</TemplateId>
                    </Operation>
                </AI_1581665960536>
                <AI_1581665960537>
                    <Type>AI</Type>
                    <Operation>
                        <TemplateId>t1460606b9752148c4ab182f55163ba7cd</TemplateId>
                    </Operation>
                </AI_1581665960537>
            </Nodes>
        </Topology>
    </IssWorkflow>
</Request>
```

### 响应

```shell
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: 230
Connection: keep-alive
Date: Thu, 15 Jun 2017 12:37:29 GMT
x-ci-request-id: NTk0MjdmODlfMjQ4OGY3XzYzYzhfMjc=

<Response>
    <IssWorkflow>
        <Name>demo</Name>
        <ProjectId></ProjectId>
        <WorkflowId></WorkflowId>
        <State></State>
        <NotifyConfig>
                <Url>http://notify.demo.com</Url>
                <Event>WorkflowFinish</Event>
                <State>On</State>
        </NotifyConfig>
        <Topology>
            <Dependencies>
                <Start>AI_1581665960536,AI_1581665960537/Start>
                <AI_1581665960536>End</AI_1581665960536>
                <AI_1581665960537>End</AI_1581665960537>
            </Dependencies>
            <Nodes>
                <Start>
                    <Type>Start</Type>
                </Start>
                <AI_1581665960536>
                    <Type>AI</Type>
                    <Operation>
                        <TemplateId>t1460606b9752148c4ab182f55163ba7cd</TemplateId>
                    </Operation>
                </AI_1581665960536>
                <AI_1581665960537>
                    <Type>AI</Type>
                    <Operation>
                        <TemplateId>t1460606b9752148c4ab182f55163ba7cd</TemplateId>
                    </Operation>
                </AI_1581665960537>
            </Nodes>
        </Topology>
        <CreateTime></CreateTime>
        <UpdateTime></UpdateTime>
    </IssWorkflow>
</Response>
```
