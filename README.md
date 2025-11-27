# LLM_VulnSeeker: 基于大模型的代码审计与漏洞挖掘实战

🚀 简介 (Introduction)
本项目记录了一次利用大型语言模型（LLM）进行代码审计和漏洞利用 Payload 调试的实战经验。核心目标是探索 LLM 在网络安全领域的赋能潜力，特别是在漏洞筛选和复杂 Payload 绕过验证环节中的交互式表现。
<img width="1416" height="837" alt="image" src="https://github.com/user-attachments/assets/d9c97d2d-3872-4bdd-8139-acc36a236de1" />




Prompt：你做为一个专业的网络安全测试员，请针对当前的项目进行分析，是否存在有典型的sql注入、代码执行、文件包含、任意代码执行，XSS，任意文件下载，逻辑漏洞，任意用户登录或者密码重置，任意文件上传等安全问题，请仔细分析，并针对性给出可利用的POC。
<img width="1404" height="867" alt="image" src="https://github.com/user-attachments/assets/f1a8e948-3fe3-41b2-a14a-b50c2cc855c5" />

第一轮结果：
模型分析完成后给出了20个可能存在的漏洞，包含各种常见的漏洞如SQL注入、代码执行、代码执行、XSS、密码重置、文件上传、认证绕过、SSRF漏洞、原型链污染漏洞、 CSRF漏洞、目录遍历漏洞、信息泄露、拒绝服务漏洞，与路径都一并给出。明显治理不高；
<img width="1401" height="1014" alt="image" src="https://github.com/user-attachments/assets/790beffe-1d1f-415d-9d84-fee72f11ac22" />


第二轮结果，要求模型进行二次的确认；
输入：请讲一些内容进行二次检查，确认是否是真实可能存在的，能够被利用的，并同时给出特定的url和利用方法，存在的漏洞名称。
模型给出了筛选后、疑似真实存在的漏洞；
1.SQL注入漏洞 ✅ 确认存在
2.任意代码执行漏洞 ✅ 确认存在
3.任意文件下载漏洞 ❌ 需要重新验证
4.XSS漏洞 ✅ 确认存在
5.密码重置绕过漏洞 ✅ 确认存在
6.文件上传漏洞 ❌ 需要重新验证
7.认证绕过漏洞 ❌ 需要重新验证
8.信息泄露漏洞 ✅ 确认存在
9.SSRF漏洞 ❌ 需要重新验证
10.原型链污染漏洞 ❌ 需要重新验证
典型案例如下：
<img width="1415" height="618" alt="image" src="https://github.com/user-attachments/assets/5e20f211-f449-43b0-9b6d-16b58dda9fc8" />
<img width="1251" height="318" alt="image" src="https://github.com/user-attachments/assets/508320e7-01df-45a0-aa50-f3a5787d2840" />


第三轮：命令执行漏洞的验证；
prompt:针对任意代码执行漏洞这个漏洞，请详细说明一下如何利用，是否可以执行系统命令。
<img width="1418" height="1032" alt="image" src="https://github.com/user-attachments/assets/486491da-9b2a-4b13-885f-096845c2b553" />

并给出了该漏洞支持的各种利用手法与攻击的payload；
<img width="1421" height="753" alt="image" src="https://github.com/user-attachments/assets/676eccaa-f5c4-47eb-a407-760448cc8c03" />

下一步给出明确的环境，给出可利用的攻击payload；
提示：已知当前测试的主机、是一个windows主机，"parseData"这个参数如何构造payload
当前模型一共给出了8种不同的利用方法与攻击payload；
<img width="2085" height="1008" alt="image" src="https://github.com/user-attachments/assets/ce824bea-264e-4a22-b39d-b5d7cacaed61" />

实际使用的过程中发现，出现了明显的报错；将明显报错的提示给到了模型；
<img width="1800" height="600" alt="image" src="https://github.com/user-attachments/assets/bf3c035d-fe15-43f9-baa0-2553f5e3643e" />

模型则开始提供一些10种不同类型、且带复杂且带绕过的攻击payload；最后也给了3个待选择的建议payload，并提供了使用的先后顺序；
<img width="1947" height="1074" alt="image" src="https://github.com/user-attachments/assets/f6b7954b-3f18-4025-96e7-c9d0e0883834" />

按照提示进行了测试、发现依然存在明显的报错，并提供的详细的报错信息；模型继续给出了新的绕过思路；一共给出了7种绕过的新方法；并给出了明确的攻击payload；
<img width="1767" height="918" alt="image" src="https://github.com/user-attachments/assets/4c555425-43dd-4899-ba68-20e426248029" />

但实际测试又出现了其他的报错信息、让我提供一些更基础的绕过方法，然后继续给出了10个绕过的方法攻击payload；
<img width="1464" height="663" alt="image" src="https://github.com/user-attachments/assets/2fa4aff8-b047-4b24-836f-e25d1e6a67e1" />

过程中多次测试报错之后，最后发现了特定的场景可以执行成功；整个过程持续了20多次反复报错确认之后，最后发现可以成功执行的攻击payload；
<img width="1422" height="807" alt="image" src="https://github.com/user-attachments/assets/481f1f61-d1ec-4a97-bde3-25ca99bb1d51" />
<img width="1422" height="705" alt="image" src="https://github.com/user-attachments/assets/f9f0a85e-000e-484c-8911-eda1bebcf55b" />
<img width="1437" height="936" alt="image" src="https://github.com/user-attachments/assets/a2ee99e6-dc15-4af8-a1a5-bb77c38b8955" />

给出简要的总结和修复方法
<img width="1419" height="666" alt="image" src="https://github.com/user-attachments/assets/b3c05143-67a6-44c9-90ff-11b33b2e49cc" />

本地复现成功
<img width="830" height="476" alt="image" src="https://github.com/user-attachments/assets/4581b527-f51c-4520-b5b1-391d4b7b1305" />



