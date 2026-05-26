---
date: '2026-05-27T03:57:15.390873'
draft: false
tags: []
title: Build an enterprise observability solution for Amazon Quick
---
# Build an enterprise observability solution for Amazon Quick

用户要求我将一个摘要扩写为完整的技术博客文章。让我先分析一下：

**输入信息**：
- 标题：Build an enterprise observability solution for Amazon Quick
- 摘要内容：关于为Amazon Quick构建企业级可观测性解决方案
- 源URL：AWS官方博客
- 风格：深度分析

**输出要求**：
1. 第一行必须是标题：`# 中文标题`
2. 禁止YAML frontmatter
3. 禁止对话痕迹
4. 正文结构：背景介绍 → 核心内容 → 技术分析 → 总结
5. 保持技术准确性
6. 翻译为中文
7. 字数：800-1500字

我需要先获取原文内容，然后扩写成完整的技术博客文章。让我先从源URL获取原文内容。我需要先获取原文内容，然后基于摘要和原文进行扩写。让我先获取文章内容。

好的，我已经获取到了原文内容。现在我可以基于摘要和原文内容来扩写成一篇完整的技术博客文章。

从原文中，我了解到：

1. **问题背景**：当数百到数千用户接入企业AI平台时，业务领导者和平台所有者需要了解：
   - 用户使用情况
   - 用户对答案的满意度
   - 哪些功能驱动了最多的参与度
   - 数据分散在多个AWS服务中，难以规模化分析

2. **Amazon Quick平台**：生成式AI平台，包含：
   - Spaces（协作空间）
   - Chat agents（对话代理）
   - Flows（流程编排）
   - Automate（自动化）
   - Research（研究）
   - Amazon Quick Sight（商业智能）

3. **解决方案架构**：
   - Amazon CloudWatch vended logs → 记录聊天对话、用户反馈、代理/研究小时使用、索引存储使用
   - AWS CloudTrail → 记录用户、角色或AWS服务在Amazon Quick中采取的操作
   - Amazon Data Firehose → 数据传输和转换
   - AWS Lambda → 数据转换
   - Amazon S3 → 数据湖存储
   - AWS Glue Data Catalog → 维护数据湖元数据
   - Amazon Athena → 查询数据
   - AWS Lake Formation → 提供细粒度的数据湖权限
   - Quick Sight dashboard → 可视化展示
   - Quick custom chat agent → 自然语言查询
   
数据安全和部署要求是实施过程中的关键考虑因素。系统采用严格的加密措施，使用AWS KMS保护静态数据，并启用自动密钥轮换。部署过程需要特定的AWS账户订阅、Python环境、Node.js框架、AWS开发工具包和命令行工具，确保安全可靠的平台搭建。# 构建 Amazon Quick 企业级可观测性解决方案

## 背景：企业AI平台规模化运营的痛点

当企业AI平台接入数百乃至数千用户时，业务领导者和平台所有者面临着一个关键挑战：缺乏对平台运营状况的集中式洞察。具体问题包括：

- **用户使用情况不明**：无法清晰了解哪些用户正在使用平台，使用频率如何
- **满意度难以量化**：用户对AI生成答案的满意程度缺乏量化指标
- **功能热度分析缺失**：哪些能力驱动了最多的用户参与度难以识别
- **数据孤岛问题**：相关运营数据分散在多个AWS服务中，无法进行规模化分析

这些问题直接影响了平台的优化方向、用户体验提升策略和投资决策，成为企业级AI平台规模化运营的核心瓶颈。

## Amazon Quick：生成式AI集成平台

Amazon Quick是一款生成式AI驱动的企业级平台，将多个核心能力整合在一处：

- **Spaces**：协作空间，支持团队知识共享与协同工作
- **Chat Agents**：对话代理，提供智能问答与交互能力
- **Flows**：流程编排，自动化业务流程管理
- **Automate**：自动化工具，提升运营效率
- **Research**：研究助手，加速知识探索
- **Amazon Quick Sight**：商业智能分析，数据可视化与洞察

随着组织扩大Amazon Quick的部署规模，需要一种可靠的方式来追踪采用率、衡量满意度、监控成本并审计治理情况——这一切需要从单一界面完成。

## 解决方案架构：构建集中式数据管道

本文提出的解决方案通过整合Amazon Quick的运营数据，构建一个安全的数据湖架构，实现统一的可观测性能力。核心架构包含以下关键组件：

### 数据源层

**Amazon CloudWatch Vended Logs**：记录聊天对话、用户反馈、代理/研究小时使用量以及索引存储使用情况。这些日志可通过数据保护策略屏蔽敏感信息（如凭证、财务数据、个人身份信息、健康信息和设备标识符）。

**AWS CloudTrail Events**：提供用户、角色或AWS服务在Amazon Quick中采取的操作记录，实现完整的审计追踪能力。

### 数据传输与处理层

**CloudWatch订阅过滤器**：将日志事件转发到Amazon Data Firehose交付流。

**Amazon Data Firehose + AWS Lambda**：Firehose交付流通过Lambda函数转换数据格式，并将其写入Amazon S3数据湖。

**Amazon EventBridge规则**：路由Amazon Quick的API调用（来自AWS CloudTrail）到专用的Firehose交付流，同样通过Lambda转换后写入数据湖。

### 数据存储与分析层

**Amazon S3数据湖**：集中存储所有运营数据，作为分析的基础。

**AWS Glue Data Catalog**：维护数据湖元数据，为Amazon Athena外部表和分析视图提供支持。

**Amazon Athena**：管理员可直接查询数据湖中的数据，进行深度分析。

**AWS Lake Formation**：提供表级和列级的细粒度数据湖权限控制，确保数据安全合规。

### 可视化与交互层

**Quick Sight仪表板**：业务领导者和利益相关者可通过交互式仪表板探索采用率、满意度、成本和治理数据。

**Quick自定义对话代理**：支持自然语言查询，用户可直接提问并获得即时可视化答案。

## 技术亮点：全链路加密与安全设计

该解决方案在安全设计上采用统一加密策略：

- **静态数据加密**：使用客户管理的AWS KMS密钥，并启用自动密钥轮换
- **多组件加密覆盖**：CloudWatch日志组、Data Firehose交付流、Lambda函数环境变量以及S3数据湖均采用加密保护
- **数据脱敏能力**：通过CloudWatch数据保护策略屏蔽敏感信息

这种全链路加密设计确保了从数据采集、传输、存储到分析的每个环节都满足企业级安全合规要求。

## 部署要求与实施路径

部署该解决方案需要以下前置条件：

- **AWS账户**：拥有Amazon Quick订阅的AWS账户
- **开发环境**：Python 3.9+、Node.js 20+、AWS CDK、AWS CLI V2
- **权限配置**：AWS CLI配置文件需具备创建IAM角色、AWS KMS密钥、CloudWatch日志组、S3存储桶、Lambda函数、Data Firehose交付流、EventBridge规则以及CloudFormation堆栈的权限
- **Lake Formation管理员权限**：如选择使用AWS Lake Formation进行数据目录访问控制，部署身份必须是Lake Formation管理员

部署过程采用分步式设计，每个步骤都基于前一步骤构建，用户可在任何步骤停止并获得该级别的工作解决方案。配置参数（如AWS CLI配置文件、资源前缀、数据库名称、工作组名称）在每一步后本地保存，后续步骤自动填充。

## 核心价值：数据驱动的平台治理

通过构建这一可观测性解决方案，企业可获得以下核心价值：

- **用户活跃度实时追踪**：清晰了解平台使用情况，识别活跃用户群体和使用模式
- **问答质量量化评估**：通过用户反馈数据衡量AI生成答案的满意度和准确性
- **功能热度智能分析**：识别最受欢迎的能力模块，指导功能优化和投资方向
- **成本监控与优化**：追踪代理使用小时、索引存储等资源消耗，实现精细化成本管理
- **治理审计合规**：完整的操作记录和权限控制，满足企业合规要求
- **自然语言交互查询**：降低数据分析门槛，业务人员可直接通过对话获得洞察

## 总结：现代AI平台治理的关键基础设施

Amazon Quick企业级可观测性解决方案解决了规模化AI运营中的"数据孤岛"问题，将分散在CloudWatch、CloudTrail等多个AWS服务中的运营数据整合到统一的数据湖中。通过Quick Sight仪表板和自定义对话代理，业务领导者可以从单一界面获得完整的平台洞察，实现数据驱动的平台优化、用户体验提升和投资决策。

这一方案不仅是技术架构的创新，更是企业AI平台治理的关键基础设施——它将可观测性能力从分散的数据采集升级为集中的智能洞察，为现代企业AI平台的规模化运营提供了坚实的数据基础。随着生成式AI在企业中的广泛应用，构建类似的可观测性系统将成为平台治理的标配能力，助力企业在AI转型中实现可控、可度量、可持续的规模化发展。


---

> 原文链接：https://aws.amazon.com/blogs/machine-learning/build-an-enterprise-observability-solution-for-amazon-quick/
