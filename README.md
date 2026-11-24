# AI_DEVELOPMENT_GUIDELINES.md

> Instruções permanentes para agentes de IA que participam dos projetos de software do proprietário.
> Prioridade: aplicar estas diretrizes em todos os projetos, salvo decisão explícita do proprietário ou instrução específica do projeto aprovada por ele.

## 1. Objetivo

Construir aplicações seguras, performáticas, escaláveis e fáceis de manter, com arquitetura simples e modular, stack consistente e agentes de IA especializados. Não substituir tecnologias por preferência própria.

## 2. Stack padrão

| Área | Padrão |
| --- | --- |
| Backend | Go (Golang) + Gin |
| Frontend | Nuxt 4.7.0 + Vue 3 |
| UI | Tailwind CSS; abordagem Mobile First |
| Renderização | SSR com Nuxt |
| Banco relacional | PostgreSQL |
| Cache | Valkey (compatível com Redis); estratégia cache-first quando apropriada |
| Arquitetura | Monólito modular, organizado por domínios |
| Repositório | Monorepo (backend e frontend) |
| Ambiente | Docker e Docker Compose |

**Regra obrigatória:** não introduzir outra linguagem, framework, banco, arquitetura ou ferramenta equivalente sem aprovação expressa do proprietário. Bibliotecas auxiliares devem ser justificadas tecnicamente e compatíveis com a stack.

## 3. Regras arquiteturais

1. Começar com monólito modular. Não criar microsserviços sem necessidade demonstrada e aprovação.
2. Definir fronteiras claras entre módulos/domínios e evitar acoplamento circular.
3. Manter lógica de negócio no backend; não confiar no frontend para autorização ou validação crítica.
4. Projetar APIs com contratos claros, validação de entrada, tratamento consistente de erros e observabilidade adequada.
5. Usar migrations PostgreSQL numeradas, versionadas e reversíveis sempre que viável; planejar mudanças seguras em tabelas grandes.
6. Considerar aplicações multiempresa (multi-tenant) quando o domínio exigir: um usuário pode administrar várias empresas; isolar dados por empresa e verificar permissões em cada operação.
7. Não adicionar abstrações, dependências ou serviços apenas por moda ou antecipação especulativa.

## 4. Frontend e segurança de integração

- Usar Nuxt 4.7.0 com SSR, Vue 3 e Tailwind CSS, com interfaces Mobile First.
- Preferir `runtimeConfig` e rotas server-side do Nuxt (`server/api`) como camada de proxy/BFF.
- **Não expor a URL interna do backend ao navegador.** Segredos e credenciais ficam exclusivamente no servidor.
- Implementar autenticação, autorização e controle de sessão conforme requisitos do projeto; não assumir um provedor sem decisão.
- Garantir estados de carregamento, erros, acessibilidade básica e responsividade.
- Validar permissões no backend, mesmo quando o frontend ocultar ações.

## 5. Cache e dados

- Considerar cache-first para leituras apropriadas, definindo explicitamente chave, escopo (incluindo tenant), TTL, invalidação, fallback e comportamento em falhas.
- Evitar vazamento de dados entre empresas por chaves de cache compartilhadas.
- PostgreSQL é a fonte de verdade, salvo especificação aprovada em contrário.
- Não usar cache em fluxos sensíveis sem analisar consistência e necessidade de atualização imediata.

## 6. Infraestrutura

- Manter `Dockerfile.dev` para desenvolvimento com hot reload.
- Manter Dockerfile de produção multi-stage, preferencialmente com imagem distroless e usuário não-root.
- Usar Docker Compose para orquestração local.
- Configurar variáveis de ambiente e segredos sem embuti-los no código ou em imagens.
- Priorizar builds reproduzíveis, health checks, logs úteis e configuração separada por ambiente.

## 7. Fluxo de trabalho esperado

1. **Entender o negócio:** esclarecer objetivo, usuários, regras e critérios de aceitação.
2. **Planejar:** identificar módulos, contratos, dados, riscos, segurança e dependências.
3. **Validar decisões:** escalar dúvidas que afetem escopo, stack, custo ou arquitetura.
4. **Delegar por especialidade:** cada agente atua apenas em seu domínio.
5. **Implementar incrementalmente:** mudanças pequenas, testáveis e integráveis.
6. **Verificar:** testes, lint, build, migrations, segurança e compatibilidade entre camadas.
7. **Entregar:** informar arquivos alterados, decisões, testes executados, pendências e próximos passos.

## 8. Regras de decisão e escalonamento

- Se a solicitação conflitar com estas diretrizes, **não trocar silenciosamente a tecnologia**: explicar o conflito, apresentar alternativas e solicitar aprovação.
- Se uma tarefa exigir habilidade fora da especialidade do agente, devolvê-la para redistribuição.
- Se faltarem requisitos essenciais, perguntar em vez de inventar regras de negócio.
- Distinguir sempre **preferências padrão** de **requisitos obrigatórios do projeto**.
- Nunca afirmar que código foi testado, implantado ou revisado sem realmente executar a verificação correspondente.
- Documentar exceções aprovadas e seu motivo.

## 9. Checklist antes de concluir uma tarefa

- [ ] Respeitei a stack definida?
- [ ] Trabalhei dentro da minha especialidade?
- [ ] Mantive os limites dos módulos e o isolamento entre empresas, se aplicável?
- [ ] Protegi segredos, autenticação e autorização?
- [ ] Evitei expor o backend interno no navegador?
- [ ] Avaliei consistência e invalidação de cache, se aplicável?
- [ ] Criei/atualizei migrations e testes necessários?
- [ ] Registrei verificações realizadas e limitações reais?
- [ ] Comuniquei decisões e bloqueios ao CTO/proprietário?

## 11. Instrução curta para inicialização de agentes

> Leia `AI_DEVELOPMENT_GUIDELINES.md` antes de planejar ou escrever código. Respeite a stack Go/Gin, Nuxt 4.7.0/Vue 3/Tailwind, PostgreSQL e Valkey; use monólito modular, SSR e Docker. Atue apenas em sua especialidade. Não mude linguagens, frameworks ou arquitetura sem aprovação explícita do proprietário. Em conflitos, informe e solicite decisão. Entregue mudanças verificáveis e reporte testes, riscos e pendências.

---

**Nota:** estas diretrizes refletem preferências recorrentes do proprietário. Requisitos específicos de um projeto podem complementá-las; exceções devem ser aprovadas explicitamente.
