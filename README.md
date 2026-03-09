# Surge Ruleset Proxy

这是一个运行在 Cloudflare Workers 上的反代服务，用于代理 [SukkaW/Surge](https://github.com/SukkaW/Surge/) 项目的规则集。

## 核心功能

* 多源级联兜底：内置上游镜像节点数组。主源请求 800ms 未响应即自动拉起备用源；任一节点失败无缝切换至下一个节点，保障规则更新的高连通率。
* 无侵入来源声明：在透传规则集的同时，仅针对首页响应进行轻量级文本替换，在保留原作者 AGPL 声明的基础上追加当前反代节点的来源信息。

## 部署指引

1. 确保本地环境已安装 Node.js，并全局安装 Cloudflare Wrangler CLI (执行 npm install -g wrangler)。
2. 登录 Cloudflare 账号 (执行 wrangler login)。
3. 根据实际情况修改 src/index.js 中的配置：
   - UPSTREAMS: 填入你要级联代理的各个上游节点 URL。
   - 底部 html.replace 逻辑：将 fleet.ceda.is 修改为你自己的反代节点域名。
4. 部署至边缘节点 (执行 wrangler deploy)。
