# MsgDistributor
根据群id/好友名/好友id动态选择Provider


## 配置

- 默认服务提供商
  - 注意：填入后会接管系统默认服务提供商，使用/provider进行的设置不再生效

- 需消息分发的群组列表
  - 格式：消息平台名::群id::服务提供商名
  - 示例：gwchat::12345678@chatroom::dify_app_vv

- 需消息分发的应用好友列表
  - 格式：消息平台名::好友id或好友名::服务提供商名
  - 示例：
    - gwchat::wxid_45526124::dify_app_vv
    - gwchat::你干嘛唉哟::dify_app_cxk

群id、好友id、好友名看log即可