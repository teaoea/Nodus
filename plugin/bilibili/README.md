# Nodus Plugin

## 哔哩哔哩 App 去广告

[插件 YAML](bilibili.plugin.yaml) 使用 Nodus Core 的本地响应和原生 JSON 复写，无需 JavaScript。

| 场景 | 行为 |
| --- | --- |
| 开屏 | 常规、活动和品牌开屏请求收到空投放列表。品牌插画也会随列表清空。 |
| 首页和竖屏推荐 | 删除带明确广告标记的卡片及广告横幅子项，保留普通推荐和未知卡片。 |
| 搜索发现 | 默认关闭。将「隐藏搜索发现页的热搜和推荐」规则的 `enable` 改为 `true` 后，仅隐藏热搜和推荐模块，保留搜索历史。 |

插件覆盖上述 App 的 JSON 接口。网页广告、视频内嵌推广和 Protobuf／gRPC 接口超出覆盖范围。

## 安装地址

将下方 URL 复制到宿主的插件地址栏，然后启用插件。

```text
https://raw.githubusercontent.com/teaoea/Nodus/main/plugin/bilibili/bilibili.plugin.yaml
```

## 加载

通过支持本地资源的宿主加载 `plugin/bilibili/bilibili.plugin.yaml`。主配置在 `plugin-providers` 中声明插件来源，宿主通过 `resources.plugins` 提供同名启用项、插件 ID 和 YAML 的绝对路径。若宿主只支持从 URL 添加插件，需使用能够返回该 YAML 原文的 HTTPS 地址。

主配置需要开启 MitM，并提供非空的 `mitm.hosts`。可登记插件涉及的 `app.bilibili.com` 和 `app.biliapi.net`。客户端需要信任宿主 CA，流量也需要经过 Core 的 MitM 入口。主配置中的 `mitm.bypass` 优先，解密按主机启用，复写按插件内列出的路径生效。

停用插件可恢复后续请求的原始行为。已缓存的开屏内容可能仍会显示，清理 B 站缓存后重新打开可验证新请求。
