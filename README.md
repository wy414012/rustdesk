# RustDesk 客户端克隆与配置指南

## 1. 克隆仓库
默认分支为 `master`，这是一个非标准测试客户端，端口不采用 `2xxxx`。

```bash
git clone --recurse-submodules <仓库地址>
```

## 2. 分支与配置
如需自定义客户端配置，请按以下步骤操作：
1. **Fork 仓库**：将仓库的`shared` 分支`Fork`到自己的账户。
2. **配置环境变量**：在 Fork 后的仓库中配置以下环境变量。

## 3. 环境变量配置
在仓库的 `settings-->secrets-->actions` 中配置以下变量按需配置：

| 变量名                  | 描述                                                                 | 示例值                                                                 |
|-------------------------|----------------------------------------------------------------------|-----------------------------------------------------------------------|
| `RS_PUB_KEY`            | 自己的服务端公钥                                                     | `11111-1111-111-111-111`                                           |
| `RENDEZVOUS_SERVER`     | 自己的 Rendezvous 服务器地址（可配置多个，用逗号分隔）               | `server1.com,server2.com`                                             |
| `API_SERVER`            | 自己的 API 服务器地址                                                | `https://api.example.com`                                            |
| `ANDROID_SIGNING_KEY`   | 安卓签名密钥                                                         | `xxxxx`                                   |
| `MACOS_P12_BASE64`      | macOS 签名证书（Base64 编码）                                        | `xxxxxx`                                     |
| `SIGN_BASE_URL`         | Windows 签名工具地址                                                 | `https://example.com/sign.exe 此变量非必须可以不用签名`                                      |  |

## 4. 注意事项
1. **端口配置**：默认端口为同步官方端口，使用 `2xxxx`,请勿克隆`master`分支来定义。
2. **多服务器支持**：`RENDEZVOUS_SERVER` 支持多个服务器地址，用逗号分隔。
3. **签名工具**：确保 `SIGN_BASE_URL` 指向有效的 Windows 签名工具地址。

---

### 补充说明
- 如果需要进一步自定义客户端行为，可以修改 `shared` 分支中的配置文件。
- 确保所有环境变量在 CI/CD 流程中正确加载。
