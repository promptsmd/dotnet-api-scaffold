# .NET Core API Scaffold — Copilot Agent Prompt

[![Abrir no GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/seu-usuario/dotnet-api-scaffold)

Prompt de agente reutilizável do GitHub Copilot para scaffolding de projetos completos de **.NET Core Web API** seguindo os princípios da **Clean Architecture**.

## Visão Geral

Este prompt configura um agente Copilot que gera soluções ASP.NET Core totalmente estruturadas a partir de uma descrição em linguagem natural. Dado um recurso ou domínio (ex.: "API de gerenciamento de produtos com CRUD completo"), o agente produz código pronto para produção em todas as camadas arquiteturais.

## O Que É Gerado

- **Estrutura da solution** com quatro projetos: `Domain`, `Application`, `Infrastructure`, `API`
- **Camada Domain** — entidades, enums, exceções de domínio, interfaces de repositório
- **Camada Application** — DTOs (usando `record`), interfaces de serviço, implementações de casos de uso, validadores FluentValidation
- **Camada Infrastructure** — EF Core `DbContext`, configuração Fluent API, migrations, implementações de repositório
- **Camada API** — controllers, middleware, métodos de extensão de DI, `Program.cs`, `appsettings.json`
- **Scaffold de testes** — stubs de projeto para testes unitários de `Domain`, `Application` e `Infrastructure`
- **`README.md`** dentro da solution gerada com tabela de endpoints, instruções de configuração e lista de pacotes NuGet
- **`.gitignore`** pré-configurado para excluir segredos, artefatos de build e arquivos de IDE

## Arquitetura Aplicada

```
NomeProjeto/
├── NomeProjeto.sln
├── NomeProjeto.Domain/          ← sem dependências internas
├── NomeProjeto.Application/     ← depende de Domain
├── NomeProjeto.Infrastructure/  ← depende de Domain + Application
├── NomeProjeto.API/             ← depende de Application + Infrastructure
└── NomeProjeto.Tests/
    ├── NomeProjeto.Domain.Tests/
    ├── NomeProjeto.Application.Tests/
    └── NomeProjeto.Infrastructure.Tests/
```

Regra de dependência: camadas internas nunca referenciam camadas externas.

## Como Usar

### 1. Pré-requisitos

- [Visual Studio Code](https://code.visualstudio.com/) com a extensão [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)
- GitHub Copilot Chat habilitado com suporte ao **modo agente**

### 2. Carregar o prompt

Abra o VS Code nesta pasta e invoque o agente pelo Copilot Chat:

```
@agent Use o agente .NET Core API Scaffold para criar uma API de gerenciamento de produtos com CRUD completo
```

Ou referencie o arquivo de prompt diretamente no Copilot Chat usando o seletor de arquivo `#`.

### 3. Descrever sua API

Forneça uma descrição em linguagem natural do domínio e das operações desejadas. Exemplos:

- *"API de gerenciamento de pedidos com clientes, produtos e itens de pedido"*
- *"API de pedidos com autenticação JWT e PostgreSQL"*
- *"API de estoque com alertas de estoque mínimo e notificações por e-mail"*

O agente faz perguntas de esclarecimento quando necessário e então gera todos os arquivos camada por camada.

## Metadados do Prompt

| Campo | Valor |
|---|---|
| **Nome** | `.NET Core API Scaffold` |
| **Arquivo** | `agent.prompt.md` |
| **Ferramentas** | `read`, `edit`, `search`, `execute`, `todo` |
| **Dica de argumento** | Descreva a API ou o recurso a ser criado |

## Convenções Aplicadas

| Aspecto | Convenção |
|---|---|
| DTOs | Tipos `record` (imutáveis) |
| Controllers | `ActionResult<T>` / `IActionResult` |
| Respostas de erro | `ProblemDetails` (RFC 7807) |
| Nomeação de rotas | Substantivos no plural em inglês — `/api/products` |
| Validação | FluentValidation |
| Configuração | Opções tipadas via `IOptions<T>` — sem valores hardcoded |
| I/O | `async/await` em todos os métodos |
| Swagger | XML comments em todos os métodos públicos dos controllers |

## Segurança

O agente aplica as seguintes restrições por padrão:

- Nenhum SQL raw sem parametrização
- Stack traces nunca expostos em respostas de produção
- Segredos e connection strings apenas via `appsettings.json` / variáveis de ambiente, nunca no código-fonte
- `.gitignore` gerado exclui: `*.pem`, `*.pfx`, `*.key`, `*service-account*.json`, `appsettings.Local.json`, `bin/`, `obj/`, `.vs/`, `.idea/`

## Licença

MIT
