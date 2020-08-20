# 多通道告警服务

```text
graph TD
A[Api Request ..]
B[Dispatch ,.]
D[Channel .]
E[Dao .]
U[User .]

A --> B
B --> D
B --> E
B --> U
```

```text
sequenceDiagram
    participant A as Client App
    participant Api as Api
    participant Ds as Dispatch
    participant Dao as Dao
    participant C as Channel
    participant User as User

    A->>Api: 发送告警数据：接收者/内容/级别/状态/指定通道等
    Api->>Ds: 数据传入调度器

    Ds->>Dao: 记录一次告警事件
    Ds->>Ds: 把告警事件接收者和通道拆分告警任务
    Ds->>Dao: 记录告警任务
    loop 发送所有告警任务
        activate User
        Ds->>User:传入用户名
        User-->>Ds:根据发送通道需要获取用户手机号或邮件
        deactivate User

        activate C
        Ds->>C:执行告警任务
        C-->>Ds:返回发送结果
        deactivate C

        Ds->>Dao:记录告警任务执行状态
    end
    Ds->>Ds: 检查告警事件下所有告警任务的发送情况
    Ds->>Dao: 记录告警事件的发送结果
    Ds-->>Api: 返回告警事件发送结果
    Api-->>A: 返回告警事件发送结果
```

