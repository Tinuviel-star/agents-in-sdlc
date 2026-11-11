---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并确认 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 为 Space Quiz 创建公开访问入口。Copilot 可以准备工作流并引导部署，但仍需审查更改并确认网站已上线。

在本课中，你将：

- 请 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已在 GitHub 存储库中，但其他人还需要一种简单的方式来试用它。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖项，即可通过公开网址访问。

## 请 Copilot 部署网站

通过提示让 Copilot 创建部署工作流并提交拉取请求。先在 `space-quiz` 的 Copilot app 会话中发送提示，再审查它提出的更改。

1. 在 Copilot app 会话中输入以下提示：

   ```plaintext
   Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
   ```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功运行的 GitHub Actions“Deploy website”工作流，其中包含“Build website”“Upload artifact”和“Deploy to Pages”步骤；以及位于 yourname.github.io/space-quiz/ 的 Space Quiz 网站。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求智能体创建并合并拉取请求，以及更改存储库的 Pages 设置。继续操作前，请审查差异、部署权限和 GitHub 的审批提示。

## 审查工作流并验证部署

1. 确认只有在存储库原本没有 `README` 时才添加 `README`。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤。
3. 只有必需的拉取请求检查通过后才合并。
4. 持续查看 GitHub Actions 部署运行情况，直到成功。
5. 打开报告中的网址，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。只有部署成功且网站正常加载后，才能将任务视为完成。

## 总结与后续步骤

Space Quiz 现已作为在线网站发布，你也已亲自验证部署，而不是只依赖智能体的报告。在本课中，你：

- 请 Copilot 配置了 GitHub Pages 部署。
- 在合并前审查了工作流和拉取请求。
- 跟进部署并验证了已发布的网站。

接下来阅读[第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
