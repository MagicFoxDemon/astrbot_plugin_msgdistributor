# MsgDistributor

根据消息平台-群id/好友名/好友id动态选择服务提供商。

# 分支说明
Astrbot 3.5.13 更新后，调用插件 `msgdistributor` 的处理函数 `on_group_message` 时出现以下报错：


:(

在调用插件 msgdistributor 的处理函数 on_group_message 时出现异常：'ProviderManager' object has no attribute 'provider_enabled'


将 `main.py` 文件中第 66 行的代码：

python
if not self.context.provider_manager.provider_enabled:
    self.context.provider_manager.provider_enabled = True


替换为：

python
if not self.context.get_using_provider():
    # 这里可以添加一些处理逻辑，例如选择一个默认的 Provider
    # 或者记录一条日志，提示用户没有启用 Provider
    logger.warning("No provider is currently enabled.")


即可正常运行。

原理不明，能用就行。

## 配置说明

### 全局设置

- `enable_distribute`: 布尔值，是否启用消息分发功能
  - `true`: 启用消息分发
  - `false`: 禁用消息分发（默认）

### Provider设置

- `default_provider`: 字符串，默认服务提供商ID
  - 当消息来源未配置特定Provider时使用
  - 留空则使用系统默认Provider
  - 注意：设置后会覆盖通过`/provider`命令设置的默认Provider

### 群组映射

- `platform_group_provider_map`: 列表，群组消息分发配置
  - 格式：`消息平台名::群id::服务提供商名`
  - 示例：`gwchat::12345678@chatroom::dify_app_vv`
  - 注意：群ID可以从日志中获取，目前只支持群ID形式

### 好友映射

- `platform_friend_provider_map`: 列表，私聊消息分发配置
  - 格式：`消息平台名::好友id或好友名::服务提供商名`
  - 示例：
    - 使用好友ID：`gwchat::wxid_45526124::dify_app_vv`
    - 使用好友名：`gwchat::你干嘛唉哟::dify_app_cxk`
  - 注意：好友ID等信息可以从日志中获取

## 配置示例

```json
{
    "enable_distribute": true,
    "default_provider": "openai_gpt4",
    "platform_group_provider_map": [
        "gwchat::12345678@chatroom::dify_app_vv",
        "qq::87654321::claude_v2"
    ],
    "platform_friend_provider_map": [
        "gwchat::wxid_45526124::dify_app_vv",
        "telegram::user123::gpt4_turbo"
    ]
}
```

## 版本历史

- v1.0.0
  - 初始发布
