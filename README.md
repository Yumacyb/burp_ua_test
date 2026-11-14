# Burp Suite 抓包与修改 User-Agent 实操笔记
## 1. 实验目的
掌握 Burp Suite 的基本代理抓包流程，学会拦截 HTTP/HTTPS 请求，并使用 Repeater（重发器）修改请求头中的 User-Agent 字段，将客户端伪装成 macOS 系统访问目标网站。
## 2. 实验环境
- 操作系统：Windows 10
- 工具：Burp Suite Professional v2.1
- 浏览器代理插件：Proxy SwitchyOmega 3
- 目标网站：北京印刷学院 (www.bigc.edu.cn)
## 3. 实验步骤
1. **配置代理与证书**：使用 SwitchyOmega 将浏览器流量指向 Burp (127.0.0.1:8080)，并安装 Burp CA 证书解决 HTTPS 警告。
2. **拦截并修改**：访问目标网站，拦截 `GET /` 请求。
   - 将 `User-Agent` 中的 `Windows NT 10.0` 修改为 `Macintosh; Intel Mac OS X 10_15_7`。
   - 将 `sec-ch-ua-platform` 修改为 `"macOS"`，保证客户端指纹一致。
3. **重发验证**：将修改后的请求发送至重发器（Repeater），点击发送。
## 4. 实验结果
*(注意：如果你传上去的图片名字不是 1.jpg，请把下面的括号里的名字改成你实际传的文件名)*
![Burp修改UA并成功返回200 OK](./burp_repeater_mac_ua 2026-11-14 175351.png.jpg)
如上图所示，左侧请求头已成功伪装为 macOS 环境，右侧服务器成功返回了 `HTTP/1.1 200 OK`，并在响应体中返回了目标网站的 HTML 源码，证明请求头修改生效。
## 5. 实验总结
- **User-Agent 的作用**：它相当于浏览器的“身份证”，服务器会根据它判断客户端环境。修改它可以绕过某些针对特定系统的限制或测试设备识别逻辑。
- **Client Hints 进阶**：现代浏览器会发送 `sec-ch-ua-platform` 头部。如果只改 UA 不改这些，容易被服务器识别出特征不一致。
- **Repeater 的价值**：使用 Repeater 可以反复修改、重放单次请求，是进行安全漏洞测试（如越权、未授权访问）的基础工具。
