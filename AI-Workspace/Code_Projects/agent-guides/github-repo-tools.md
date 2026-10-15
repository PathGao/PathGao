# GitHub 项目辅助阅读工具

用户偏好：拿 GitHub 项目来讨论时，按问题选用 GitDiagram、DeepWiki、Gitingest 辅助解释。下列链接格式与用途于 2026-09-29 对照官方说明核对，未逐一运行具体仓库。

以 `https://github.com/owner/repo` 为例，`owner/repo` 替换为实际仓库路径。

| 编号 | 工具 | 转换后的地址 | 用途与边界 |
| --- | --- | --- | --- |
| T1 | GitDiagram | `https://gitdiagram.com/owner/repo` | 生成交互式架构图，辅助理解模块关系。 |
| T2 | DeepWiki | `https://deepwiki.com/owner/repo` | 查看 AI 生成的项目百科、架构说明并提问，未收录项目可能需要先索引。 |
| T3 | Gitingest | `https://gitingest.com/owner/repo` | 将仓库整理为适合模型阅读的文本，可按问题筛选文件范围。 |
| T4 | GitHub1s | `https://github1s.com/owner/repo` | 用类似 VS Code 的网页界面浏览源码。 |
| T5 | github.dev | `https://github.dev/owner/repo` | 在线编辑代码，不提供构建、运行代码所需的计算环境或终端。 |
| T6 | StackBlitz | `https://stackblitz.com/github/owner/repo` | 导入并在线运行兼容项目，使用前检查技术栈、依赖和启动配置，不能保证任意 GitHub 项目都能运行。 |

## 使用约定

1. 看整体结构时选 GitDiagram，需要按主题理解时选 DeepWiki，需要集中阅读相关源码时选 Gitingest。已有源码足够回答时直接阅读，不强制绕经第三方服务。
2. 图和百科属于辅助材料。解释关键调用关系、设计原因或行为前，对照目标分支或提交的 README 与源码，说明核对的版本。无法核实时明确缺口，不把生成内容当作已验证事实。
3. 工具不可用或资料过期时，回到仓库文档与源码继续。默认仅使用公开仓库；向第三方提供私有源码或凭据需有对应授权。
4. GitHub1s、github.dev 和 StackBlitz 留作浏览、编辑、运行入口。解释请求本身不授权修改、提交代码或运行项目。

## 官方说明

- [GitDiagram](https://github.com/ahmedkhaleel2004/gitdiagram)
- [DeepWiki](https://cognition.com/blog/deepwiki)
- [Gitingest](https://github.com/coderamp-labs/gitingest)
- [GitHub1s](https://github.com/conwnet/github1s)
- [github.dev](https://docs.github.com/en/codespaces/the-githubdev-web-based-editor)
- [StackBlitz GitHub 导入](https://developer.stackblitz.com/guides/integration/open-from-github)
