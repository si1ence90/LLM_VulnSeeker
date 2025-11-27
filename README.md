# LLM_VulnSeeker

> 基于大语言模型的代码审计与漏洞挖掘实战项目

## 📖 项目简介

本项目记录了一次利用大型语言模型（LLM）进行代码审计和漏洞利用 Payload 调试的实战经验。核心目标是探索 LLM 在网络安全领域的赋能潜力，特别是在漏洞筛选和复杂 Payload 绕过验证环节中的交互式表现。

![项目概览](https://github.com/user-attachments/assets/d9c97d2d-3872-4bdd-8139-acc36a236de1)

## ✨ 核心特性

- 🔍 **智能漏洞识别**：利用 LLM 进行多轮代码审计，识别常见安全漏洞
- 🎯 **精准漏洞筛选**：通过二次验证机制，提高漏洞识别的准确性
- 🛠️ **Payload 生成**：自动生成针对性的漏洞利用 Payload
- 🔄 **交互式调试**：支持多轮交互，逐步优化 Payload 绕过验证
- 📊 **漏洞分类**：支持 SQL 注入、代码执行、XSS、文件上传等多种漏洞类型

## 🚀 使用流程

### 第一轮：初始漏洞扫描

**Prompt 示例：**
```
你作为一个专业的网络安全测试员，请针对当前的项目进行分析，是否存在有典型的sql注入、代码执行、文件包含、任意代码执行，XSS，任意文件下载，逻辑漏洞，任意用户登录或者密码重置，任意文件上传等安全问题，请仔细分析，并针对性给出可利用的POC。
```

![第一轮扫描结果](https://github.com/user-attachments/assets/f1a8e948-3fe3-41b2-a14a-b50c2cc855c5)

**结果分析：**
- 模型分析完成后给出了 **20 个可能存在的漏洞**
- 包含各种常见漏洞类型：SQL 注入、代码执行、XSS、密码重置、文件上传、认证绕过、SSRF、原型链污染、CSRF、目录遍历、信息泄露、拒绝服务等
- 初步识别准确率较低，需要进一步验证

![第一轮详细结果](https://github.com/user-attachments/assets/790beffe-1d1f-415d-9d84-fee72f11ac22)

### 第二轮：漏洞二次验证

**Prompt 示例：**
```
请将这些内容进行二次检查，确认是否是真实可能存在的，能够被利用的，并同时给出特定的url和利用方法，存在的漏洞名称。
```

**验证结果：**

| 漏洞类型 | 状态 | 说明 |
|---------|------|------|
| SQL 注入漏洞 | ✅ 确认存在 | 已验证可被利用 |
| 任意代码执行漏洞 | ✅ 确认存在 | 已验证可被利用 |
| 任意文件下载漏洞 | ❌ 需要重新验证 | 需进一步确认 |
| XSS 漏洞 | ✅ 确认存在 | 已验证可被利用 |
| 密码重置绕过漏洞 | ✅ 确认存在 | 已验证可被利用 |
| 文件上传漏洞 | ❌ 需要重新验证 | 需进一步确认 |
| 认证绕过漏洞 | ❌ 需要重新验证 | 需进一步确认 |
| 信息泄露漏洞 | ✅ 确认存在 | 已验证可被利用 |
| SSRF 漏洞 | ❌ 需要重新验证 | 需进一步确认 |
| 原型链污染漏洞 | ❌ 需要重新验证 | 需进一步确认 |

![第二轮验证结果](https://github.com/user-attachments/assets/5e20f211-f449-43b0-9b6d-16b58dda9fc8)
![漏洞详情](https://github.com/user-attachments/assets/508320e7-01df-45a0-aa50-f3a5787d2840)

### 第三轮：深度利用验证

#### 3.1 代码执行漏洞分析

**Prompt 示例：**
```
针对任意代码执行漏洞这个漏洞，请详细说明一下如何利用，是否可以执行系统命令。
```

![代码执行漏洞分析](https://github.com/user-attachments/assets/486491da-9b2a-4b13-885f-096845c2b553)

**利用方法：**
模型提供了多种利用手法与攻击 Payload：

![Payload 生成](https://github.com/user-attachments/assets/676eccaa-f5c4-47eb-a407-760448cc8c03)

#### 3.2 环境特定 Payload 生成

**Prompt 示例：**
```
已知当前测试的主机是一个 Windows 主机，"parseData" 这个参数如何构造 payload
```

**生成结果：**
- 模型共给出了 **8 种不同的利用方法与攻击 Payload**

![Windows 环境 Payload](https://github.com/user-attachments/assets/ce824bea-264e-4a22-b39d-bb7cacaed61)

#### 3.3 错误处理与绕过

**问题：** 实际测试过程中出现报错

![报错信息](https://github.com/user-attachments/assets/bf3c035d-fe15-43f9-baa0-2553f5e3643e)

**解决方案：**
- 模型提供了 **10 种不同类型且带复杂绕过的攻击 Payload**
- 给出了 3 个待选择的建议 Payload，并提供了使用的先后顺序

![绕过方案](https://github.com/user-attachments/assets/f6b7954b-3f18-4025-96e7-c9d0e0883834)

**持续优化：**
- 继续报错后，模型提供了 **7 种新的绕过方法**

![新绕过方法](https://github.com/user-attachments/assets/4c555425-43dd-4899-ba68-20e426248029)

- 再次报错后，模型提供了 **10 个基础绕过方法的攻击 Payload**

![基础绕过方法](https://github.com/user-attachments/assets/2fa4aff8-b047-4b24-836f-e25d1e6a67e1)

#### 3.4 成功利用

经过 **20+ 次反复报错确认** 后，最终发现了可以成功执行的攻击 Payload：

![成功执行](https://github.com/user-attachments/assets/481f1f61-d1ec-4a97-bde3-25ca99bb1d51)
![执行结果](https://github.com/user-attachments/assets/f9f0a85e-000e-484c-8911-eda1bebcf55b)
![详细输出](https://github.com/user-attachments/assets/a2ee99e6-dc15-4af8-a1a5-bb77c38b8955)

## 📋 漏洞总结与修复建议

模型在成功利用后，自动给出了简要的总结和修复方法：

![修复建议](https://github.com/user-attachments/assets/b3c05143-67a6-44c9-90ff-11b33b2e49cc)

## ✅ 本地复现

本地环境成功复现漏洞利用：

![本地复现](https://github.com/user-attachments/assets/4581b527-f51c-4520-b5b1-391d4b7b1305)

## 🎯 项目亮点

1. **多轮交互验证**：通过多轮 Prompt 交互，逐步提高漏洞识别的准确性
2. **智能 Payload 生成**：根据环境特点自动生成针对性的攻击 Payload
3. **错误自适应**：能够根据报错信息自动调整和优化 Payload
4. **实战验证**：所有漏洞均经过实际环境验证，确保可复现

⭐ 如果这个项目对你有帮助，请给它一个 Star！
