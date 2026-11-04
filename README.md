# agent2api-plugin-index

`weiang12345/agent2api` 私有源码仓库配套的**公开**更新索引仓库。

网关的「软件更新」和「插件页」都从这个仓库取索引：

- `app.json` —— 网关自身从哪个仓库找 Release（`app.repo`）。
- `index.json` —— 插件列表（`plugins` 数组）。
- `plugins/<id>.json` —— 单个插件的详情（`release.repo` / `release.tag` / `release.asset`）。

索引里的仓库地址必须与源码里 `desktop-tauri/src-tauri/server/src/server/core/update/version.rs`
的 `DEFAULT_REPO` 一致；两处不一致的症状是「Release 建出来了，但没有人收到更新」。

## 文件说明

```
app.json                            网关更新源
index.json                          插件索引（当前为空，见下）
plugins/agent2api-providers.json         提供商增强插件详情模板
plugins/agent2api-web-providers.json     网页提供商插件详情模板
```

## 当前状态

`index.json` 的 `plugins` 是**空数组**。

原因是我们还没有产出任何 `-fork.N` 的插件包：一旦把插件挂进索引，网关就会按
`release.tag` 去找对应的 Release 资产，资产不存在时插件页会显示安装失败。
所以正确顺序是：

1. 先把 `weiang12345/agent2api-providers-releases` 里对应 tag 的 zip 和签名发出来；
2. 再把 `plugins/*.json` 里的 `version`、`platforms` 对齐到本次实际构建结果；
3. 最后才把条目加进 `index.json` 的 `plugins` 数组。

## 命名规则

- 插件 id 只允许小写字母、数字和 `-`。
- 插件包资产名固定为 `{id}-{version}-{target}.zip`，target 用标准 Rust 三元组。
- 本 fork 额外发 `armv7-unknown-linux-gnu`（上游只发 x86_64 / aarch64）。
- 网关版本 tag 用 `v{version}`；插件版本 tag 用 `v{version}`，网页插件用 `web-v{version}`。

## 与上游的关系

上游对应的索引仓库是 `Agent2plugin/Agent2plugin`。本仓库是它的 fork：
结构保持兼容，只把仓库地址、版本号和平台列表换成本 fork 的值。
上游结构变化时按同样思路合并，不另起一套格式。
