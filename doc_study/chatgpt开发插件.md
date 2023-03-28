## 插件开发申请地址：

https://openai.com/waitlist/plugins



 ChatGPT 智能调用我们写的 API 的插件。

OpenAI 插件将 ChatGPT 连接到第三方应用程序。这些插件使 ChatGPT 能够与开发人员定义的 API 进行交互，从而增强 ChatGPT 的功能并允许其执行范围广泛的操作。

- 插件可以允许 ChatGPT 执行以下操作：
- 检索实时信息；例如，体育比分、股票价格、最新消息等。
- 检索知识库信息；例如，公司文件、个人笔记等。
- 代表用户执行操作；例如，订机票、订餐等。



# 官方文档开发教程：

## 插件流程介绍

要构建插件，了解**端到端流程**很重要。

1. **创建清单文件**并将其托管在 yourdomain.com/.well-known/ai-plugin.json
   - 该文件包括**插件的元数据（名称、徽标等）**、有关所需身份验证的详细信息（身份验证类型、OAuth URL 等）以及公开的端点的 OpenAPI 规范。
   - 该模型将看到 OpenAPI 描述字段，可用于为不同字段提供自然语言描述。
   - 开始时仅公开 1-2 个端点，并使用最少数量的参数来最小化文本的长度。插件描述、API 请求和 API 响应都被插入到与 ChatGPT 的对话中。这不利于模型的上下文限制。
2. [在ChatGPT UI](https://chat.openai.com/)中注册插件
   - 从顶部下拉菜单中选择插件型号，然后选择“插件”、“插件商店”，最后选择“安装未经验证的插件”或“开发您自己的插件”。
   - 可能需要身份验证，请提供 OAuth 2 client_id 和 client_secret 或 API 密钥
3. 用户激活您的插件
   - 用户必须在 ChatGPT UI 中手动激活您的插件。（默认情况下，ChatGPT 不会使用您的插件。）
   - 在 alpha 期间，插件开发者将能够与 15 个额外的用户共享他们的插件（目前只有其他开发者可以安装未经验证的插件）。
   - 如果需要身份验证，用户将通过 OAuth 重定向到您的插件；您也可以选择在这里创建新帐户。
4. 用户开始对话
   - OpenAI 将在发送给 ChatGPT 的消息中插入对您的插件的简洁描述，最终用户看不到。这将包括插件描述、端点和示例。
   - 当用户提出相关问题时，如果看起来相关，模型可能会选择从您的插件调用 API 调用；对于 POST 请求，我们要求开发人员构建用户确认流程。
   - 该模型会将 API 结果合并到其对用户的响应中。
   - 该模型可能在其响应中包含从 API 调用返回的链接。这些将显示为丰富的预览（使用[OpenGraph 协议](https://ogp.me/)，我们在其中提取站点名称、标题、描述、图像和 url 字段）”





## 插件入门

创建插件需要三个步骤：

- 构建 API
- 以 OpenAPI yaml 或 JSON 格式记录 API
- 创建一个 JSON 清单文件（/.well-known/ai-plugin.json），它将定义插件的相关元数据

### 插件清单

每个插件都需要一个`ai-plugin.json`文件，该文件需要托管在 API 的域中。例如，一家名为 example.com 的公司将使插件 JSON 文件可通过[https://example.com](https://example.com/)域访问，因为这是托管他们的 API 的地方。当您通过 ChatGPT UI 安装插件时，我们会在后端查找位于`/.well-known/ai-plugin.json`. 该`/.well-known`文件夹是必需的，并且必须存在于您的域中，以便 ChatGPT 与您的插件连接。如果找不到文件，则无法安装插件。对于本地开发，您可以使用 HTTP，但如果您指向远程服务器，则需要 HTTPS。

所需文件的最小定义`ai-plugin.json`如下所示：

```json

{
  "schema_version": "v1",
  "name_for_human": "TODO Plugin",
  "name_for_model": "todo",
  "description_for_human": "Plugin for managing a TODO list. You can add, remove and view your TODOs.",
  "description_for_model": "Plugin for managing a TODO list. You can add, remove and view your TODOs.",
  "auth": {
    "type": "none"
  },
  "api": {
    "type": "openapi",
    "url": "http://localhost:3333/openapi.yaml",
    "is_user_authenticated": false
  },
  "logo_url": "http://localhost:3333/logo.png",
  "contact_email": "support@example.com",
  "legal_info_url": "http://www.example.com/legal"
}
```

如果您想查看插件文件的所有可能选项，您可以参考下面的定义。

| 场地                  | 类型                  | 描述/选项                                                    |
| :-------------------- | :-------------------- | :----------------------------------------------------------- |
| 架构版本              | 细绳                  | 清单架构版本                                                 |
| 名称_for_model        | 细绳                  | 命名模型将用于定位插件                                       |
| name_for_human        | 细绳                  | 人类可读的名称，例如完整的公司名称                           |
| description_for_model | 细绳                  | 更适合模型的描述，例如令牌上下文长度注意事项或用于改进插件提示的关键字使用。 |
| description_for_human | 细绳                  | 插件的人类可读描述                                           |
| 授权                  | 清单认证              | 身份验证架构                                                 |
| 应用程序接口          | 目的                  | API规范                                                      |
| logo_url              | 细绳                  | 用于获取插件徽标的 URL                                       |
| 联系电子邮件          | 细绳                  | 用于安全/审核联系、支持和停用的电子邮件联系人                |
| 法律信息网址          | 细绳                  | 重定向 URL 供用户查看插件信息                                |
| HttpAuthorizationType | HttpAuthorizationType | “承载”或“基本”                                               |
| 清单授权类型          | 清单授权类型          | “无”、“user_http”、“service_http”或“oauth”                   |
| 接口 BaseManifestAuth | BaseManifestAuth      | 类型：ManifestAuthType；说明：字符串；                       |
| 清单无授权            | 清单无授权            | 无需身份验证：BaseManifestAuth & { type: 'none', }           |
| 清单认证              | 清单认证              | ManifestNoAuth、ManifestServiceHttpAuth、ManifestUserHttpAuth、ManifestOAuthAuth |

以下是使用不同身份验证方法的示例：

```typescript

# App-level API keys
type ManifestServiceHttpAuth  = BaseManifestAuth & {
  type: 'service_http';
  authorization_type: HttpAuthorizationType;
  verification_tokens: {
    [service: string]?: string;
  };
}

# User-level HTTP authentication
type ManifestUserHttpAuth  = BaseManifestAuth & {
  type: 'user_http';
  authorization_type: HttpAuthorizationType;
}

type ManifestOAuthAuth  = BaseManifestAuth & {
  type: 'oauth';

  # OAuth URL where a user is directed to for the OAuth authentication flow to begin.
  client_url: string;

  # OAuth scopes required to accomplish operations on the user's behalf.
  scope: string;

  # Endpoint used to exchange OAuth code with access token.
  authorization_url: string;

  # When exchanging OAuth code with access token, the expected header 'content-type'. For example: 'content-type: application/json'
  authorization_content_type: string;

  # When registering the OAuth client ID and secrets, the plugin service will surface a unique token. 
  verification_tokens: {
    [service: string]?: string;
  };
}
```

清单文件中某些字段的长度也有一些限制，这些限制可能会随着时间的推移而变化：

- 最多 50 个字符`name_for_human`
- 最多 50 个字符`name_for_model`
- 最多 120 个字符`description_for_human`
- 最多 8000 个字符`description_for_model`（会随着时间的推移而减少）

另外，我们对 API 响应正文长度也有 100k 个字符的限制（会随着时间的推移而减少），这也可能会发生变化。

#### OpenAPI 定义

下一步是构建[OpenAPI 规范](https://swagger.io/specification/)以记录 API。除了 OpenAPI 规范和清单文件中定义的内容外，ChatGPT 中的模型对您的 API 一无所知。这意味着，如果您拥有广泛的 API，则无需向模型公开所有功能，并且可以选择特定的端点。例如，如果您有一个社交媒体 API，您可能希望模型通过 GET 请求访问网站内容，但要阻止模型对用户帖子发表评论，以减少垃圾邮件的可能性。

OpenAPI 规范是位于 API 之上的包装器。基本的 OpenAPI 规范如下所示：



```text
openapi: 3.0.1
info:
  title: TODO Plugin
  description: A plugin that allows the user to create and manage a TODO list using ChatGPT.
  version: 'v1'
servers:
  - url: http://localhost:3333
paths:
  /todos:
    get:
      operationId: getTodos
      summary: Get the list of todos
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/getTodosResponse'
components:
  schemas:
    getTodosResponse:
      type: object
      properties:
        todos:
          type: array
          items:
            type: string
          description: The list of todos.
```

我们首先定义规范版本、标题、描述和版本号。在 ChatGPT 中运行查询时，它将查看信息部分中定义的描述，以确定插件是否与用户查询相关。[您可以在写作说明](https://platform.openai.com/docs/plugins/getting-started/writing-descriptions)部分阅读更多关于提示的信息。

请记住您的 OpenAPI 规范中的以下限制，这些限制可能会发生变化：

- API 规范中的每个 API 端点描述/摘要字段最多 200 个字符
- API 规范中每个 API 参数描述字段最多 200 个字符

由于我们在本地运行此示例，因此我们希望将服务器设置为指向您的本地主机 URL。OpenAPI 规范的其余部分遵循传统的 OpenAPI 格式，您可以通过各种在线资源[了解有关 OpenAPI 格式的更多信息](https://swagger.io/tools/open-source/getting-started/)。还有许多工具可以根据您的底层 API 代码自动生成 OpenAPI 规范。

#### 运行插件

为您的 API 创建 API、清单文件和 OpenAPI 规范后，您现在就可以通过 ChatGPT UI 连接插件了。您的插件可能在两个不同的地方运行，要么在本地开发环境中，要么在远程服务器上。

如果您有本地版本的 API 正在运行，您可以将插件接口指向该本地设置。要将插件与 ChatGPT 连接，您可以导航到插件商店，然后选择“安装未经验证的插件”。

如果插件在远程服务器上运行，您需要先选择“开发自己的插件”，然后选择“安装未经验证的插件”。您只需将插件清单文件添加到 ./well-known 路径并开始测试您的 API。但是，对于清单文件的后续更改，您必须将新更改部署到您的公共站点，这可能需要很长时间。在这种情况下，我们建议设置一个本地服务器作为您的 API 的代理。这使您可以快速对 OpenAPI 规范和清单文件的更改进行原型设计。

设置公共 API 的本地代理

#### 撰写说明

当用户发出可能是对插件的潜在请求的查询时，模型会查看 OpenAPI 规范中端点的描述以及`description_for_model`清单文件中的 。就像提示其他语言模型一样，您需要测试多个提示和描述，看看哪种效果最好。

OpenAPI 规范本身是一个很好的地方，可以为模型提供有关 API 各种细节的信息——可用的功能、参数等。除了为每个字段使用富有表现力、信息丰富的名称外，规范还可以包含“描述”每个属性的字段。例如，这些可用于提供功能的功能或查询字段期望的信息的自然语言描述。该模型将能够看到这些，它们将指导它使用 API。如果一个字段仅限于某些值，您还可以提供一个带有描述性类别名称的“枚举”。

“description_for_model”属性让您可以自由地指导模型一般如何使用您的插件。总体而言，ChatGPT 背后的语言模型能够高度理解自然语言并遵循指令。因此，这是一个很好的地方，可以放置有关插件功能以及模型应如何正确使用它的一般说明。使用自然语言，最好使用简洁但具有描述性和客观性的语气。您可以查看一些示例以了解这应该是什么样子。我们建议从`description_for_model`“Plugin for …”开始，然后枚举您的 API 提供的所有功能。

#### 最佳实践

以下是在 OpenAPI 规范中编写`description_for_model`和描述以及设计 API 响应时要遵循的一些最佳实践：

1. 您的描述不应试图控制 ChatGPT 的情绪、个性或确切响应。ChatGPT 旨在为插件编写适当的响应。

   *坏例子*：

   > 当用户要求查看他们的待办事项列表时，请始终回复“我能够找到您的待办事项列表！您有[x]待办事项：[在此处列出待办事项] 。如果您愿意，我可以添加更多待办事项！”

   *很好的例子*：

   > [不需要说明]

2. 当用户没有要求您的插件的特定服务类别时，您的描述不应鼓励 ChatGPT 使用该插件。

   *坏例子*：

   > 每当用户提到任何类型的任务或计划时，询问他们是否愿意使用 TODOs 插件将某些内容添加到他们的待办事项列表中。

   *很好的例子*：

   > TODO列表可以添加、删除和查看用户的TODO。

3. 您的描述不应规定 ChatGPT 使用该插件的特定触发器。ChatGPT 旨在在适当的时候自动使用您的插件。

   *坏例子*：

   > 当用户提到一个任务时，回复“你想让我把它添加到你的 TODO 列表中吗？说‘是’继续。”

   *很好的例子*：

   > [不需要说明]

4. 除非必要，否则插件 API 响应应返回原始数据而不是自然语言响应。ChatGPT 将使用返回的数据提供自己的自然语言响应。

   *坏例子*：

   > 我找到了你的待办事项列表！你有两个待办事项：买杂货和遛狗。如果你愿意，我可以添加更多待办事项！

   *很好的例子*：

   > {“todos”：[“买杂货”，“遛狗”] }

#### 调试

默认情况下，聊天不会显示插件调用和其他未向用户显示的信息。为了更完整地了解模型如何与您的插件交互，您可以在与插件交互后单击插件名称上的向下箭头来查看请求和响应。

对插件的模型调用通常包括来自模型（“助手”）的消息，其中包含发送到插件的类似 JSON 的参数，然后是来自插件（“工具”）的响应，最后是来自利用插件返回的信息的模型。

在某些情况下，例如在插件安装期间，错误可能会出现在浏览器的 javascript 控制台中。

### 插件认证

插件提供多种身份验证模式以适应各种用例。要为您的插件指定身份验证模式，请使用清单文件。我们的[插件域政策](https://platform.openai.com/docs/plugins/production/domain-verification-and-security)概述了我们解决域安全问题的策略。有关可用身份验证选项的示例，请参阅[示例部分](https://platform.openai.com/docs/plugins/examples)，其中展示了所有不同的选择。

#### 无认证

我们支持不需要身份验证的应用程序的无身份验证流程，用户可以不受任何限制地直接向您的 API 发送请求。如果您有一个开放的 API 并希望所有人都可以使用，这将特别有用，因为它允许来自 OpenAI 插件请求以外的其他来源的流量。



```json
1
2
3
"auth": {
  "type": "none"
},
```

#### 服务水平

如果你想专门启用 OpenAI 插件来使用你的 API，你可以在插件安装流程中提供客户端密码。这意味着来自 OpenAI 插件的所有流量都将经过身份验证，但不会在用户级别进行身份验证。此流程受益于简单的最终用户体验，但从 API 的角度来看控制较少。

- 首先，开发人员粘贴他们的访问令牌（全局密钥）
- 然后，他们必须将验证令牌添加到他们的清单文件中
- 我们存储令牌的加密版本
- 用户在安装插件时不需要做任何事情
- 最后，我们在向插件发出请求时将其传递到 Authorization 标头中（“Authorization”: “ [Bearer/Basic][user's token] ”）



```json
1
2
3
4
5
6
7
"auth": {
  "type": "service_http",
  "authorization_type": "bearer",
  "verification_tokens": {
    "openai": "cb7cdfb8a57e45bc8ad7dea5bc2f8324"
  }
},
```

#### 用户等级

就像用户可能已经在使用您的 API 一样，我们允许最终用户在插件安装期间将他们的秘密 API 密钥复制并粘贴到 ChatGPT UI 中，从而实现用户级身份验证。虽然我们在将密钥存储在数据库中时对其进行了加密，但鉴于用户体验不佳，我们不推荐这种方法。

- 首先，用户在安装插件时粘贴他们的访问令牌
- 我们存储令牌的加密版本
- 然后我们在向插件发出请求时将其传递到 Authorization 标头中（“Authorization”：“ [Bearer/Basic][user's token] ”）



```json
1
2
3
4
"auth": {
  "type": "user_http",
  "authorization_type": "bearer",
},
```

#### OAuth

插件协议与 OAuth 兼容。我们在清单中期望的 OAuth 流程的一个简单示例如下所示：

- 首先，开发人员粘贴他们的 OAuth 客户端 ID 和客户端密码
  - 然后他们必须将验证令牌添加到他们的清单文件中
- 我们存储客户端密钥的加密版本
- 用户在安装插件时通过插件的网站登录
  - 这为我们提供了用户的 OAuth 访问令牌（以及可选的刷新令牌），我们将其加密存储
- 最后，我们在向插件发出请求时在 Authorization 标头中传递该用户的令牌（“Authorization”：“ [Bearer/Basic][user's token] ”）



```json
"auth": {
  "type": "oauth",
  "client_url": "https://my_server.com/authorize",
  "scope": "",
  "authorization_url": "https://my_server.com/token",
  "authorization_content_type": "application/json",
  "verification_tokens": {
    "openai": "abc123456"
  }
},
```

为了更好地理解 OAuth 的 URL 结构，这里是字段的简短描述：

- 当您使用 ChatGPT 设置插件时，系统会要求您提供 OAuth`client_id`和`client_secret`
- 当用户登录插件时，ChatGPT 会将用户的浏览器定向到`"[client_url]?response_type=code&client_id=[client_id]&scope=[scope]&redirect_uri=https%3A%2F%2Fchat.openai.com%2Faip%2F[plugin_id]%2Foauth%2Fcallback"`
- 在您的插件重定向回给定的 redirect_uri 后，ChatGPT 将通过`authorization_url`使用内容类型`authorization_content_type`和参数发出 POST 请求来完成 OAuth 流程`{ “grant_type”: “authorization_code”, “client_id”: [client_id], “client_secret”: [client_secret], “code”: [the code that was returned with the redirect], “redirect_uri”: [the same redirect uri as before] }`

### 示例插件

为了开始构建，我们提供了一组涵盖不同身份验证模式和用例的简单插件。从我们简单的无身份验证待办事项列表插件到更强大的检索插件，这些示例让我们得以一窥我们希望通过插件实现的目标。

[在开发过程中，您可以在本地计算机上](https://platform.openai.com/docs/plugins/getting-started/running-a-plugin)或通过 GitHub Codespaces、Replit 或 CodeSandbox 等云开发环境运行插件。

### 生产中的插件

[速率限制](https://platform.openai.com/docs/plugins/production/rate-limits)

考虑对您公开的 API 端点实施速率限制。虽然目前规模有限，但 ChatGPT 被广泛使用，您应该会收到大量请求。您可以监控请求的数量并相应地设置限制。

[更新你的插件](https://platform.openai.com/docs/plugins/production/updating-your-plugin)

在插件获得批准并可供 ChatGPT 用户访问后，您可能需要随时更新清单文件。ChatGPT 将在发出请求时定期获取 OpenAPI 规范。要手动更新清单文件，请导航至插件商店并再次执行“开发您自己的插件”流程。

[插件条款](https://platform.openai.com/docs/plugins/production/plugin-terms)

要注册插件，您必须同意[插件条款](http://openai.com/policies/plugin-terms)。

[域验证和安全](https://platform.openai.com/docs/plugins/production/domain-verification-and-security)

为确保插件只能对它们控制的资源执行操作，OpenAI 对插件的清单和 API 规范强制执行要求。

[定义插件的根域](https://platform.openai.com/docs/plugins/production/defining-the-plugin-s-root-domain)

清单文件定义了向用户显示的信息（如徽标和联系信息）以及托管插件的 OpenAPI 规范的 URL。获取清单时，将按照以下规则建立插件的根域：

- 如果该域有一个子域，那么根域将从托管清单的域中`www.`剥离出来。`www.`
- 否则，根域与托管清单的域相同。

关于重定向的注意事项：如果在解析清单时有任何重定向，则只允许子域重定向。唯一的例外是从 www 子域重定向到没有 www 的子域。

根域的示例：

- ✅

  ```
  https://example.com/.well-known/ai-plugin.json
  ```

  - 根域：`example.com`

- ✅

  ```
  https://www.example.com/.well-known/ai-plugin.json
  ```

  - 根域：`example.com`

- ✅ 

  ```
  https://www.example.com/.well-known/ai-plugin.json
  ```

  → 重定向到

  ```
  https://example.com/.well-known/ai-plugin.json
  ```

  - 根域：`example.com`

- ✅ 

  ```
  https://foo.example.com/.well-known/ai-plugin.json
  ```

  → 重定向到

  ```
  https://bar.foo.example.com/.well-known/ai-plugin.json
  ```

  - 根域：`bar.foo.example.com`

- ✅ 

  ```
  https://foo.example.com/.well-known/ai-plugin.json
  ```

  → 重定向到

  ```
  https://bar.foo.example.com/baz/ai-plugin.json
  ```

  - 根域：`bar.foo.example.com`

- ❌ 

  ```
  https://foo.example.com/.well-known/ai-plugin.json
  ```

  → 重定向到

  ```
  https://example.com/.well-known/ai-plugin.json
  ```

  - 不允许重定向到父级域

- ❌ 

  ```
  https://foo.example.com/.well-known/ai-plugin.json
  ```

  → 重定向到

  ```
  https://bar.example.com/.well-known/ai-plugin.json
  ```

  - 不允许重定向到同一级别的子域

- ❌ 

  ```
  https://example.com/.well-known/ai-plugin.json
  ```

  -> 重定向到

  ```
  https://example2.com/.well-known/ai-plugin.json
  ```

  - 不允许重定向到另一个域

[清单验证](https://platform.openai.com/docs/plugins/production/manifest-validation)

清单本身中的特定字段必须满足以下要求：

- `api.url`- 提供给 OpenAPI 规范的 URL 必须托管在同一级别或根域的子域中。
- `legal_info`-提供的URL的[二级域名](https://en.wikipedia.org/wiki/Second-level_domain)必须与根域名的二级域名相同。
- `contact_info`- 邮件地址的二级域名应与根域的二级域名相同。

[解析 API 规范](https://platform.openai.com/docs/plugins/production/resolving-the-api-spec)

清单中的字段`api.url`提供了指向 OpenAPI 规范的链接，该规范定义了插件可以调用的 API。OpenAPI 允许指定多个[服务器基础 URL](https://swagger.io/docs/specification/api-host-and-base-path/)。以下逻辑用于选择服务器 URL：

- 如果存在其域与根域完全匹配的服务器 URL，则使用该 URL
- 否则，使用作为根域子域的第一个服务器 URL。
- 如果以上两种情况都不适用，则默认为托管 API 规范的域。例如，如果规范托管在 上`api.example.com`，则将`api.example.com`用作 OpenAPI 规范中路由的基本 URL。

注意：请避免使用重定向来托管 API 规范和任何 API 端点，因为不能保证始终遵循重定向。

[使用 TLS 和 HTTPS](https://platform.openai.com/docs/plugins/production/use-tls-and-https)

使用插件的所有流量（例如，获取`ai-plugin.json`文件、OpenAPI 规范、API 调用）必须使用 TLS 1.2 或更高版本以及有效的公共证书。

[IP 出口范围](https://platform.openai.com/docs/plugins/production/ip-egress-ranges)

[ChatGPT 将从CIDR 块](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) 中的 IP 地址调用您的插件`23.102.140.112/28`。您可能希望将这些 IP 地址明确列入白名单。

对于 OpenAI 的网页浏览插件，将通过 IP 地址块调用网站`23.98.142.176/28`。





# youtube开发指南：

参考YouTube：https://www.youtube.com/watch?v=hpePPqKxNq8

国内镜像：https://www.bilibili.com/video/BV1c84y1g7Qv/?spm_id_from=333.880.my_history.page.click





## fork官方插件仓库

将官方chatgpt_plugins的github作为分支成为我们的仓库。

![image-20230327210632982](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230327210632982.png)

为此我们将转到开放式AI 聊天 GPT 检索插件存储库，拉取代码到本地文件夹，进入这个文件夹，可以看到目录基本上所有这个插件都将成为一个docker容器，我们正在部署一个API，我们将允许chat GPT 与之交互

## vscode 打开目录

![image-20230327211208123](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230327211208123.png)

主要注意服务器内部拥有的东西。

存在 upsert和query这些接口。

![image-20230327211353795](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230327211353795.png)



## langchain这个插件是怎么接入并使用的。

upset和query对api进行交互，这个API 后面的查询会有一个矢量数据库 这是一个 Pinecone组件，采用我们的线链点 lang chain

通过整个代理事务，让我们的大型语言模型能够访问langchain这个文档

![image-20230328113137546](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230328113137546.png)

获取全部的langchain文档 通过python 笔记本运行它们

运行下载，并将透明扔进upsert端点

它会在另一边出现并进入一个开放的 AI 嵌入模型

![image-20230328113434078](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230328113434078.png)

langchain的数据都存储在pinecone向量数据库，这些事ChatGPT在这的潜在信息来源

建立chatgpt与pinecone之间的连接：

用到query端点

就是询查，比如会问的线性链中的 LLM 链是什么？

![image-20230328114349739](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230328114349739.png)



![image-20230327212157166](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230327212157166.png)

现在chatgpt就可以回答这个问题了

API就是实现Chatgpt与外部世界互动的关键



## deploy：

www.digitalocean.com

### 1.使用DigitalOcean

![image-20230328114802720](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230328114802720.png)



### 2.创建web应用

![image-20230328114921902](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230328114921902.png)

create-->apps

使用github来创建资源和源代码

需要身份验证

选择仓库为从opanai的那里复制的代码库。

![image-20230328115505768](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230328115505768.png)



### 环境变量：

6个变量，1.插件密钥 2.chatgpt api 密钥 3.pinecone向量数据库

![image-20230328115738919](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230328115738919.png)

- BEARER_TOKEN 是我们插件的密码令牌，用来验证传入请求。
- OPENAI_API_KEY openai的密钥chatgpt的api密钥
- DATASTORE 数据源使用pinecone
- PINECONE_API_KEY
- PINECONE_ENVIRONMENT
- PINECONE_INDEX

BEARER_TOKE：

一般使用BEARER_TOKEN 使用JWT JSON WEB TOKENS  进入jwt.io官方网站生成就好

PINECONE：

![image-20230328120615121](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230328120615121.png)



回顾之前的绘图：

![image-20230328121026294](chatgpt%E5%BC%80%E5%8F%91%E6%8F%92%E4%BB%B6.assets/image-20230328121026294.png)

回顾之前的第一步，就是将line chain文档**通过python代码发送给api**在传输给pinecone

这就i是我们现在要做的。

