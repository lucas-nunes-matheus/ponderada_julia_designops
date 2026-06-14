<Table>
  <tr>
    <td><a href="https://asisprojetos.com.br/asis-quem-somos/"><img src="img/logo.png" alt="Logo ASIS" border="0"></td>
    <td>
      <a href="https://www.inteli.edu.br/"><img src="img/logo-Inteli.png" alt="Inteli - Instituto de Tecnologia e Liderança" border="0"></a>
    </td>
  </tr>
</table>

# Nome do Projeto: Esteira Ágil de Software

## Nome do Grupo: Kombi com Asas

## Integrantes:

- <a href="https://www.linkedin.com/in/odanielaugusto/">Daniel Augusto</a>
- <a href="https://www.linkedin.com/in/joao-souza-campos/">João Victor de Souza Campos</a>
- <a href="https://www.linkedin.com/in/lucas-nunes-matheus/">Lucas Matheus Nunes</a>
- <a href="https://www.linkedin.com/in/omatheusrsantos/">Matheus Ribeiro dos Santos</a>
- <a href="https://www.linkedin.com/in/paulo-henrique-ribeiro-5b8794243">Paulo Henrique Ribeiro</a>
- <a href="https://www.linkedin.com/in/thiagogomesalmeida/">Thiago Gomes</a>
- <a href="https://www.linkedin.com/in/thiagovolcati/">Thiago Volcati</a>
- <a href="https://www.linkedin.com/in/vinicius-ibiapina/">Vinicius Ibiapina</a>

# Sumário

- [1. Introdução e Objetivo](#designops-introducao)
- [2. Como Trabalhamos Juntos (Pessoas)](#designops-pessoas)
- [3. Como o Trabalho é Feito (Processos e Ferramentas)](#designops-processos)
- [4. Como Geramos Impacto (Mensuração e Definition of Done)](#designops-impacto)
- [5. Integração com a Esteira de CI/CD](#designops-cicd)
- [6. Gestão de Design Debt e Sustentabilidade da Esteira](#designops-divida)
- [7. Versionamento Semântico do Design System e Gestão de Changelog](#designops-semver)
- [8. Dependências entre interfaces, dados e a esteira](#designops-dados-esteira)
- [9. Referências Gerais](#designops-referencias)

--

<a id="designops-introducao"></a>

# 1. Introdução e Objetivo

DesignOps é, na prática, o DevOps do design: a disciplina que organiza pessoas, processos e ferramentas para que decisões visuais cheguem ao código sem atrito. Segundo o Nielsen Norman Group, DesignOps tem três eixos (como trabalhamos juntos, como entregamos trabalho e como geramos impacto) e é exatamente essa estrutura que orienta este documento.

No contexto do nosso projeto (uma pipeline de qualidade de código de software para os drones da Jacto), o papel do DesignOps é específico: usar as ferramentas de design para minimizar o esforço do usuário em adaptar o ativo atual da Jacto ao Horizonte Digital que estamos propondo com essa pipeline. Em outras palavras, o design entra como redutor de fricção de adoção: faz com que a transição do fluxo legado para o novo ambiente seja perceptivelmente leve, e não um custo extra empurrado para quem já opera o sistema. Dois desdobramentos práticos sustentam isso: garantir que protótipos do Figma virem componentes reais no frontend sem retrabalho e que mudanças visuais não quebrem builds já validados no pipeline de CI/CD.

**Referência:** Nielsen Norman Group, [Design Operations 101](https://www.nngroup.com/articles/design-operations-101/).

**Responsável:** Matheus Ribeiro dos Santos

<a id="designops-pessoas"></a>

# 2. Como Trabalhamos Juntos (Pessoas)

## Papéis

A estrutura de pessoas reflete o papel do design como redutor de fricção de adoção. Cada papel existe para garantir que o novo ambiente proposto pelo Horizonte Digital seja percebido como leve pelo operador da Jacto, não como mais uma ferramenta a aprender.

| Papel | Responsabilidade |
| --- | --- |
| UX/UI Lead | Garante que os componentes no Figma reflitam o fluxo real do operador, não uma visão idealizada do sistema |
| Dev Lead | Consome os tokens de design e garante que a implementação seja fiel ao que foi validado com o usuário |
| DevOps Engineer | Automatiza a publicação do Storybook e os testes de regressão visual, mantendo a esteira livre de regressões visuais silenciosas |

## Rituais

**Design Sync semanal (30 min):** alinha o que está em design com o que está em desenvolvimento, com foco em identificar pontos onde a interface pode estar gerando atrito desnecessário na transição do fluxo legado.

**Handover via Figma + Issue no GitLab:** toda mudança de interface abre uma issue descrevendo o que muda, por quê e qual comportamento do usuário essa mudança endereça.

**Review de MR com screenshot:** antes de aprovar um merge request com mudanças visuais, o revisor compara o layout com o protótipo aprovado para garantir que a intenção de design chegou ao código.

Essa estrutura segue o modelo de colaboração contínua do *Figma Design Systems Handbook*, que recomenda reduzir reuniões longas em favor de artefatos assíncronos bem documentados, o que é especialmente relevante num projeto onde o contexto de uso (drones em campo) exige decisões rápidas e rastreáveis.

**Referência:** Figma, [Design Systems Handbook](https://www.designbetter.co/design-systems-handbook).

**Responsável:** Vinicius Ibiapina

<a id="designops-processos"></a>

# 3. Como o Trabalho é Feito (Processos e Ferramentas)

Se o objetivo do DesignOps neste projeto é tornar a transição do fluxo legado para o Horizonte Digital perceptivelmente leve para o operador da Jacto, as ferramentas escolhidas precisam refletir essa mesma lógica internamente: zero retrabalho entre quem projeta e quem implementa.

## Ferramentas

O Figma centraliza os protótipos e componentes, sempre orientados ao fluxo real do operador, não a uma visão idealizada do sistema. Os Design Tokens (cores, tipografia, espaçamentos) vivem em `src/tokens/` como variáveis CSS e são exportados via plugin Style Dictionary em formato JSON. Essa camada é a fonte única de verdade entre design e código: decisões visuais tomadas no Figma, inclusive as que reduzem a fricção da transição do fluxo legado, se propagam de forma consistente por todo o frontend, sem que cada desenvolvedor precise reinterpretar a intenção do designer. O Storybook documenta os componentes em produção e serve de referência viva tanto para o time quanto para stakeholders da Jacto. A esteira de automação roda no GitLab CI.

## Fluxo Design para Código

```text
Figma (protótipo validado com foco na adoção do operador)
  → Export de Design Tokens (JSON via Style Dictionary)
  → Commit na branch de feature
  → GitLab CI: lint de CSS + build do Storybook
  → MR com screenshot de comparação
  → Merge na main → publicação automática do Storybook
```

Cada etapa desse fluxo existe para eliminar uma categoria específica de atrito. O lint de CSS impede que tokens sejam ignorados silenciosamente no código. O build do Storybook detecta componentes quebrados antes de qualquer deploy. O screenshot no MR garante que a intenção de design chegou ao código sem distorção. A publicação automática do Storybook elimina a ambiguidade sobre qual versão do componente está em uso, o que é crítico num ambiente onde interface e operação de campo estão diretamente conectadas.

Essa estrutura segue as recomendações do *Figma Design Systems Handbook* de priorizar artefatos assíncronos bem documentados em vez de ciclos longos de aprovação, alinhado a um contexto de uso que exige decisões rápidas e rastreáveis.

**Referência:** Salesforce, [Lightning Design System: Design Tokens](https://www.lightningdesignsystem.com/design-tokens/).

**Responsável:** Paulo Henrique Ribeiro

<a id="designops-impacto"></a>

# 4. Como Geramos Impacto (Mensuração e Definition of Done)

O impacto do DesignOps neste projeto é medido pela eficiência da interface em suportar operações críticas sem introduzir erro humano. Como a Jacto transita de máquinas físicas para sistemas autônomos, o design deve garantir que o "Horizonte Digital" seja tão confiável quanto o hardware legado. Conforme a metodologia do Nielsen Norman Group, a maturidade do DesignOps é atingida quando as métricas de design focam em agilidade e qualidade técnica, e não apenas em estética. Uma entrega de interface só é considerada concluída se facilitar a governança e a agilidade do fluxo de engenharia.

## Definition of Done (DoD) de Design

Para que um incremento de interface seja aceito no pipeline, ele deve cumprir:

- [ ] **Validação Técnica:** protótipo revisado pelo Dev Lead para garantir viabilidade de implementação via tokens.
- [ ] **Integridade Visual:** tokens de design exportados e em conformidade com o Style Dictionary.
- [ ] **Acessibilidade Operacional:** contraste e legibilidade validados para ambientes de campo (padrão WCAG AA).
- [ ] **Documentação Viva:** componente funcional registrado no Storybook.
- [ ] **Gate de Qualidade:** teste de regressão visual (BackstopJS) aprovado, garantindo que novos ajustes não quebraram fluxos de controle de voo ou telemetria já existentes.

## Métricas de Sucesso (KPIs)

Diferente de métricas de vaidade, focamos em indicadores que refletem a saúde da operação:

**Taxa de Erro na Promoção de Release:** mede se a interface do dashboard permitiu que um firmware inválido fosse "promovido" por ambiguidade visual. A meta é zero.

**Cycle Time de Handover:** tempo decorrido entre a aprovação do protótipo no Figma e a disponibilidade do componente no Storybook. O objetivo é reduzir o atrito entre o design e a prontidão do código.

**Cobertura de Design System:** porcentagem das telas do sistema que utilizam componentes padronizados. Alta cobertura indica menor risco de comportamentos inesperados na interface.

**Usabilidade de Gatekeeper:** tempo que um engenheiro leva para identificar um "Gargalo de Segurança" no dashboard. Conforme planejado nos testes não funcionais, o alvo é o reconhecimento em menos de 10 segundos.

**Responsável:** Thiago Gomes

<a id="designops-cicd"></a>

# 5. Integração com a Esteira de CI/CD

Para integrar os times de Desenvolvimento, de Design e de Operações de TI, é necessário entender o “horizonte” tecnológico do parceiro. Atualmente a Jacto Drones atua com um time de desenvolvimento pequeno (cerca de 2 engenheiros, 1 gerente de projetos e 1 especialista de produto). O projeto desenvolvido pelo grupo no Inteli precisa considerar a integração no ambiente do parceiro e a futura experiência do desenvolvedor (“DevEx”) como base para uma adaptação e adoção bem-sucedidas da pipeline CI/CD entregue.

Adotamos práticas de DesignOps integradas ao ciclo de vida de desenvolvimento de software (SDLC) como ganho real para o parceiro. Enquanto não existe um profissional dedicado só a DesignOps, o time prepara uma pipeline DevOps que integra produto, desenvolvimento de software e design, para que o *onboarding* de um designer futuro seja bem-sucedido.

No cenário atual, DevOps somado a design reduz o *overhead* operacional do time pequeno: os desenvolvedores podem focar mais no desenvolvimento do que em etapas repetitivas (por exemplo, desenvolvimento, integração e testes A/B de um novo gráfico no dashboard de controle de drones), porque o fluxo prioriza codificar componente e avaliar métricas de negócio em tempo real, em vez de redefinir estilo a cada iteração, testar manualmente cada versão da UI ou subir atualização na nuvem sem padrão.

Assim, a pipeline CI/CD entra como geradora de valor na proposta do projeto entregue pelo grupo no Inteli.

## Estrutura da pipeline

Pensando no contexto do Inteli e no ambiente da Jacto Drones, propomos uma esteira para reduzir sobrecarga operacional e manter consistência de forma automatizada. Nos baseamos na ideia do Salesforce Lightning Design System (SLDS): diferente de um guia de estilos estático, o SLDS é uma plataforma agnóstica de framework (React, Vue, etc.), com design distribuído via Design Tokens.

Considerando a necessidade de processar logs de telemetria e construir interfaces para simuladores da Jacto Drones, a esteira CI/CD deve tratar o design como código versionado. Etapas sugeridas:

**Etapa 1: Commit (“The Single Source of Truth”)**  
O designer atualiza um componente ou token no Figma (por exemplo, escala de um gráfico de telemetria) e o Figma, via plugin, exporta um arquivo `tokens.json`. O *push* desse JSON para o repositório central do Design System no GitHub/GitLab dispara a pipeline.

**Etapa 2: Build de Estilos**  
A pipeline executa uma ferramenta de *build* e o `tokens.json` vira variáveis consumíveis pelas aplicações web (CSS ou outros formatos acordados pelo time).

**Etapa 3: Testes Automatizados**  
A pipeline carrega a biblioteca de componentes e executa *linters* para garantir que o código gerado segue os padrões definidos com a Jacto.

**Etapa 4: Distribuição de Pacotes (Registry)**  
Após aprovação nos testes, a pipeline gera uma nova versão do pacote de design e publica em um registry acordado (por exemplo, npm privado), quando aplicável ao desenho real do parceiro.

**Etapa 5: Deploy e Monitoramento de Impacto**  
O código da aplicação web com atualização da UI segue para o ambiente definido. Product Owner e designer acompanham se a nova interface reduziu tempo de análise de falhas ou outra métrica de negócio acordada.

**Referências:** Salesforce, [Lightning Design System: Design Tokens](https://www.lightningdesignsystem.com/design-tokens/).

**Responsável:** Lucas Matheus Nunes

<a id="designops-divida"></a>

# 6. Gestão de Design Debt e Sustentabilidade da Esteira (Maturidade Operacional)

Em um ecossistema de missão crítica como o da Jacto Drones, a pressa na entrega de novas funcionalidades para firmware ou plataformas pode gerar o que o Nielsen Norman Group chama de *design debt* (dívida de design). Se componentes de interface são criados sob demanda sem seguir tokens ou o Design System, a esteira de CI/CD fica mais lenta e o custo de manutenção sobe.

Esta seção estabelece o protocolo operacional para identificar, mensurar e pagar essa dívida, garantindo que a infraestrutura de design acompanhe a evolução do Horizonte Digital da Jacto sem comprometer a confiabilidade operacional.

## Protocolos de Sustentabilidade

**Identificação de design bugs:** todo desvio visual detectado pelo BackstopJS que for aprovado em caráter excepcional (para agilizar um deploy crítico) deve gerar automaticamente uma issue com a *label* `design-debt` no GitLab.

**Refatoração de tokens:** trimestralmente, o time revisa a “fuga de tokens” (uso de cores *hardcoded* não detectadas pelo lint) para manter a fonte única de verdade íntegra.

**Impacto no *value stream*:** reduzir dívida de design encurta o *cycle time* das próximas sprints, pois componentes limpos e documentados no Storybook permitem integrar novas camadas (dados, telemetria) sem retrabalho de interface.

## Métricas de Maturidade

Para áreas comerciais e de RevOps, a saúde da esteira pode ser reportada com:

**Design Debt Ratio:** relação entre issues de novas *features* versus issues de refatoração de design.

**Tempo de Recuperação (MTTR visual):** tempo médio para corrigir um componente quebrado na esteira após falha em regressão visual.

**Responsável:** Daniel Augusto

<a id="designops-semver"></a>

# 7. Versionamento Semântico do Design System e Gestão de Changelog

Em um pipeline de CI/CD para sistemas críticos como os da Jacto Drones, todo artefato precisa ser versionado, inclusive artefatos de design. Uma mudança em token de cor ou de espaçamento pode quebrar um componente com impacto semelhante ao de uma mudança de API que quebra o *build*. A ausência de disciplina de versionamento no design system é um dos caminhos para regressões visuais silenciosas e correções emergenciais em produção.

Esta seção define a estratégia de versionamento e *changelog* que conecta mudanças de design à automação da pipeline, com rastreabilidade e controle sobre o ciclo de vida do design system.

## Versionamento semântico aplicado ao design

Adotamos o [Semantic Versioning 2.0.0](https://semver.org/) para o pacote do design system, com o seguinte mapeamento:

| Tipo | Exemplo | O que inclui |
| --- | --- | --- |
| **MAJOR** | `1.x.x` → `2.x.x` | Renomeação de tokens, remoção de componentes, mudança de “API visual”. Exige guia de migração. |
| **MINOR** | `1.1.x` → `1.2.x` | Novos tokens, novas variantes, novos componentes. Retrocompatível. |
| **PATCH** | `1.1.1` → `1.1.2` | Correções visuais que não alteram o contrato (ex.: ajuste de 1px em espaçamento). |

Essa convenção é familiar ao time de desenvolvimento da Jacto, que já versiona firmware e APIs com SemVer, o que reduz a curva de aprendizado para a mesma disciplina no design.

## Changelog automatizado via Conventional Commits

Todo *commit* que toque `src/tokens/` ou componentes do design system deve seguir a especificação [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/). A pipeline pode executar [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog) a cada merge na `main` e gerar entrada em `CHANGELOG.md`, reduzindo a ambiguidade de “qual versão está aprovada?” quando designer e desenvolvedor divergem sobre o estado atual.

Exemplos:

```text
feat(tokens): add telemetry-alert color scale         → MINOR
fix(tokens): correct contrast ratio on status-danger  → PATCH
feat(button)!: rename variant 'ghost' to 'outlined'   → MAJOR (breaking)
```

## Política de breaking changes

Mudanças que quebram compatibilidade do design system disparam um fluxo de depreciação:

1. O token ou componente antigo recebe anotação `@deprecated` no código e aviso no Storybook.
2. O lint (`stylelint`) bloqueia o uso de tokens depreciados em código novo.
3. O item permanece por **um ciclo de sprint** antes da remoção definitiva.
4. A remoção gera bump **MAJOR** e entrada obrigatória no `CHANGELOG.md` com instrução de migração.

Esse fluxo espelha práticas de compatibilidade retroativa que a engenharia da Jacto já aplica em APIs de firmware, reduzindo atrito de adoção e tornando quebras visíveis antes de produção.

## Por que isso importa no contexto da Jacto

Num sistema em que a interface precisa operar com confiabilidade na estação de controle em campo e exibir telemetria em tempo real, um design system sem versão é risco operacional. Renomeação não rastreada de token pode alterar silenciosamente a hierarquia visual de um alerta crítico, com impacto além de “bug de UX”. O versionamento é a ponte entre intenção de design e confiabilidade operacional.

**Responsável:** Thiago Volcati

<a id="designops-dados-esteira"></a>

# 8. Dependências entre interfaces, dados e a esteira

O desenho da interface do nosso cenário não vive só de intenção visual. Parte das telas que imaginamos para o Horizonte Digital da Jacto, principalmente dashboards ligados a telemetria, simulação e leitura de logs, só ganha sentido quando existe dado com formato estável para mostrar. Na prática isso cria uma fila que não aparece no desenho do componente, mas aparece no GitLab: o desenvolvedor precisa de *payload* de exemplo, o designer precisa validar leitura em condição adversa (sol forte, campo, pressa de operação) e o pipeline precisa rodar sem depender de base proprietária ou de voo real, o que o escopo acadêmico nem sempre permite.

Por isso adotamos uma regra simples de operação, alinhada ao que o Nielsen Norman Group costuma preconizar para times ágeis: reduzir dependência oculta entre pesquisa, design e entrega, deixando explícito o que está “pronto para integrar” versus o que ainda está “esperando dado ou cenário”. Ver [UX Research in Agile](https://www.nngroup.com/articles/ux-research-agile/). Em termos de DesignOps, isso vira **contrato de trabalho**, não opinião. Quando uma história de interface exige série temporal ou alerta de missão, a issue no GitLab deve declarar no corpo: origem do dado de apoio (*mock* versionado, amostra anonimizada ou *fixture* gerada pelo time), responsável por atualizar esse *mock* quando o contrato mudar, e o critério visual mínimo que o merge precisa provar no Storybook. Assim o CI continua como portão de qualidade sem transformar o designer em gargalo nem empurrar para o desenvolvedor a adivinhação de formato.

O fluxo técnico conversa com a seção 3: tokens e componentes seguem caminho conhecido. A novidade aqui é a **camada de dados de apoio ao design**, versionada como qualquer outro artefato que dispara etapa na esteira. O Storybook é onde o componente “respira” com dados plausíveis; o merge request traz captura comparando layout e **estado preenchido** que o operador veria. A documentação do Storybook sobre testes e CI reforça tratar documentação interativa como parte do produto ([Writing Tests](https://storybook.js.org/docs/writing-tests/)).

Para o negócio da Jacto, o ganho é menos retrabalho quando a equipe interna vê a tela pela primeira vez com dados verossímeis, e menos risco de promover uma UI que “funciona vazia” mas falha sob carga ou sob formato real de log. Para o time pequeno descrito na seção 5, o ganho é tempo: menos *mock* refeito em canal informal e menos MR reaberto por JSON de exemplo desatualizado.

Em suma, DesignOps também orquestra a fila de dependências de conteúdo e de dados, não só pixels e tokens. Sem isso, a automação vira coreografia em que parte do time não entra em cena.

**Referências:** Nielsen Norman Group, [UX Research in Agile](https://www.nngroup.com/articles/ux-research-agile/); Storybook, [Writing tests](https://storybook.js.org/docs/writing-tests/).

**Responsável:** João Victor de Souza Campos

<a id="designops-referencias"></a>

# 9. Referências Gerais

- Nielsen Norman Group: [Design Operations 101](https://www.nngroup.com/articles/design-operations-101/)
- Medium, Designer Hangout: [What is design operations and why should you care](https://medium.com/designer-hangout/what-is-design-operations-and-why-should-you-care-b72f02b477761)
- Figma: [Design Systems Handbook](https://www.designbetter.co/design-systems-handbook)
- Salesforce: [Lightning Design Tokens](https://www.lightningdesignsystem.com/design-tokens/)
- BackstopJS: [Visual regression testing](https://github.com/garris/BackstopJS)
- Storybook: [Continuous integration](https://storybook.js.org/docs/writing-tests/)
- Stylelint: [Get started](https://stylelint.io/user-guide/get-started)
- UX Collective: [What is design debt and why you should treat it seriously](https://uxdesign.cc/what-is-design-debt-and-why-you-should-treat-it-seriously-4366d33d3c89)
- Semantic Versioning: [semver.org](https://semver.org/)
- Conventional Commits: [conventionalcommits.org](https://www.conventionalcommits.org/en/v1.0.0/)
- conventional-changelog: [GitHub](https://github.com/conventional-changelog/conventional-changelog)
- Nielsen Norman Group: [UX Research in Agile](https://www.nngroup.com/articles/ux-research-agile/)
