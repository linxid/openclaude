---
title: "Claude Code on Microsoft Foundry"
description: "了解如何通过 Microsoft Foundry 配置 Claude Code，包括设置、配置和故障排除。"
---

## 前置条件

在使用 Microsoft Foundry 配置 Claude Code 之前，请确保您拥有：

* 具有 Microsoft Foundry 访问权限的 Azure 订阅
* 创建 Microsoft Foundry 资源和部署的 RBAC 权限
* 已安装并配置 Azure CLI（可选 - 仅在您没有其他获取凭证机制时需要）

## 设置

### 1. 配置 Microsoft Foundry 资源

首先，在 Azure 中创建 Claude 资源：

1. 导航到 [Microsoft Foundry 门户](https://ai.azure.com/)
2. 创建新资源，记下您的资源名称
3. 为 Claude 模型创建部署：
   * Claude Opus
   * Claude Sonnet
   * Claude Haiku

### 2. 配置 Azure 凭证

Claude Code 支持两种 Microsoft Foundry 身份验证方法。选择最适合您安全要求的方法。

**选项 A：API 密钥身份验证**

1. 在 Microsoft Foundry 门户中导航到您的资源
2. 转到**端点和密钥**部分
3. 复制 **API 密钥**
4. 设置环境变量：

```bash  theme={null}
export ANTHROPIC_FOUNDRY_API_KEY=your-azure-api-key
```

**选项 B：Microsoft Entra ID 身份验证**

当未设置 `ANTHROPIC_FOUNDRY_API_KEY` 时，Claude Code 会自动使用 Azure SDK [默认凭证链](https://learn.microsoft.com/en-us/azure/developer/javascript/sdk/authentication/credential-chains#defaultazurecredential-overview)。
这支持多种方法来验证本地和远程工作负载。

在本地环境中，您通常可以使用 Azure CLI：

```bash  theme={null}
az login
```

<Note>
  使用 Microsoft Foundry 时，`/login` 和 `/logout` 命令被禁用，因为身份验证通过 Azure 凭证处理。
</Note>

### 3. 配置 Claude Code

设置以下环境变量以启用 Microsoft Foundry。请注意，您的部署名称设置为 Claude Code 中的模型标识符（如果使用建议的部署名称，可能是可选的）。

```bash  theme={null}
# 启用 Microsoft Foundry 集成
export CLAUDE_CODE_USE_FOUNDRY=1

# Azure 资源名称（将 {resource} 替换为您的资源名称）
export ANTHROPIC_FOUNDRY_RESOURCE={resource}
# 或提供完整的基础 URL：
# export ANTHROPIC_FOUNDRY_BASE_URL=https://{resource}.services.ai.azure.com

# 将模型设置为您的资源的部署名称
export ANTHROPIC_DEFAULT_SONNET_MODEL='claude-sonnet-4-5'
export ANTHROPIC_DEFAULT_HAIKU_MODEL='claude-haiku-4-5'
export ANTHROPIC_DEFAULT_OPUS_MODEL='claude-opus-4-1'
```

有关模型配置选项的更多详情，请参阅[模型配置](/zh-CN/model-config)。

## Azure RBAC 配置

`Azure AI User` 和 `Cognitive Services User` 默认角色包括调用 Claude 模型所需的所有权限。

对于更严格的权限，请创建具有以下内容的自定义角色：

```json  theme={null}
{
  "permissions": [
    {
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/providers/*"
      ]
    }
  ]
}
```

有关详情，请参阅 [Microsoft Foundry RBAC 文档](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/rbac-azure-ai-foundry)。

## 故障排除

如果您收到错误"Failed to get token from azureADTokenProvider: ChainedTokenCredential authentication failed"：

* 在环境中配置 Entra ID，或设置 `ANTHROPIC_FOUNDRY_API_KEY`。

## 其他资源

* [Microsoft Foundry 文档](https://learn.microsoft.com/en-us/azure/ai-foundry/what-is-azure-ai-foundry)
* [Microsoft Foundry 模型](https://ai.azure.com/explore/models)
* [Microsoft Foundry 定价](https://azure.microsoft.com/en-us/pricing/details/ai-foundry/)
