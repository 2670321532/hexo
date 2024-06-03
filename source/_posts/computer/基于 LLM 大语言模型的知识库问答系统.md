---
title: 基于 LLM 大语言模型的知识库问答系统
date: 2024/6/2 21:41:25
comments: true
tags: MaxKB 
categories: 
- 计算机
- 项目
- web
- 人工智能
- MaxKB 
---

项目运行示例

**http://www.feelapple.cn:5002/**

MaxKB 是一款基于 LLM 大语言模型的知识库问答系统。MaxKB = Max Knowledge Base，旨在成为企业的最强大脑。

- **开箱即用**：支持直接上传文档、自动爬取在线文档，支持文本自动拆分、向量化、RAG（检索增强生成），智能问答交互体验好；
- **无缝嵌入**：支持零编码快速嵌入到第三方业务系统；
- **多模型支持**：支持对接主流的大模型，包括 Ollama 本地私有大模型（如 Meta Llama 3、qwen 等）、通义千问、OpenAI、Azure OpenAI、Kimi、智谱 AI、讯飞星火和百度千帆大模型等。

## 快速开始

```
docker run -d --name=maxkb -p 8080:8080 -v ~/.maxkb:/var/lib/postgresql/data 1panel/maxkb

# 用户名: admin
# 密码: MaxKB@123..
```

## 添加 Ollama

```
基础模型
wizardlm2
API 域名
http://host.docker.internal:11434
API Key
随便填
```



你也可以通过 [1Panel 应用商店](https://apps.fit2cloud.com/1panel) 快速部署 MaxKB + Ollama + Llama 2，30 分钟内即可上线基于本地大模型的知识库问答系统，并嵌入到第三方业务系统中。

如果是内网环境，推荐使用 [离线安装包](https://community.fit2cloud.com/#/products/maxkb/downloads) 进行安装部署。

你也可以在线体验：[DataEase 小助手](https://dataease.io/docs/v2/)，它是基于 MaxKB 搭建的智能问答系统，已经嵌入到 DataEase 产品及在线文档中。

如你有更多问题，可以查看使用手册，或者通过论坛与我们交流。

- [使用手册](https://github.com/1Panel-dev/MaxKB/wiki/1-安装部署)
- [演示视频](https://www.bilibili.com/video/BV1BE421M7YM/)
- [论坛求助](https://bbs.fit2cloud.com/c/mk/11)
- 技术交流群

## 技术栈

- 前端：[Vue.js](https://cn.vuejs.org/)
- 后端：[Python / Django](https://www.djangoproject.com/)
- LangChain：[LangChain](https://www.langchain.com/)
- 向量数据库：[PostgreSQL / pgvector](https://www.postgresql.org/)
- 大模型：Azure OpenAI、OpenAI、百度千帆大模型、[Ollama](https://github.com/ollama/ollama)、通义千问、Kimi、智谱 AI、讯飞星火

## 我们的其他明星开源项目



- [1Panel](https://github.com/1panel-dev/1panel/) - 现代化、开源的 Linux 服务器运维管理面板
- [Halo](https://github.com/halo-dev/halo/) - 强大易用的开源建站工具
- [JumpServer](https://github.com/jumpserver/jumpserver/) - 广受欢迎的开源堡垒机
- [DataEase](https://github.com/dataease/dataease/) - 人人可用的开源数据可视化分析工具
- [MeterSphere](https://github.com/metersphere/metersphere/) - 现代化、开源的测试管理及接口测试工具