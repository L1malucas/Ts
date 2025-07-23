# Análise de Requisitos - Guia Completo

## Sumário

1.  Introdução
2.  Conceitos Fundamentais
3.  Tipos de Requisitos
4.  Processo de Análise
5.  Técnicas de Levantamento
6.  Documentação de Requisitos
7.  Validação e Verificação
8.  Casos Práticos
9.  Erros Comuns e Como Evitar
10. Templates e Ferramentas

---

## 1. Introdução

### O que é Análise de Requisitos?

**Análise de Requisitos** é o processo de descobrir, analisar, documentar e verificar os serviços requeridos de um sistema e as restrições sob as quais ele deve operar. É a fundação de qualquer projeto de software bem-sucedido.

### Por que é Importante?

**Estatísticas do setor mostram que:**

- 70% dos projetos de software falham devido a requisitos mal definidos
- Corrigir um erro de requisito na fase de desenvolvimento custa 10x mais que na fase de análise
- Corrigir após o deploy pode custar até 100x mais

**Benefícios da boa análise:**

- Reduz retrabalho e custos
- Melhora a satisfação do cliente
- Diminui riscos do projeto
- Facilita estimativas precisas
- Melhora a qualidade do produto final

### Papel do Analista de Requisitos

O analista atua como **ponte** entre:

- **Stakeholders** (quem define o que precisa ser feito)
- **Equipe técnica** (quem implementa a solução)

**Responsabilidades principais:**

- Entender o problema de negócio
- Identificar todos os stakeholders
- Extrair e documentar requisitos
- Validar entendimento com stakeholders
- Comunicar requisitos para a equipe técnica
- Gerenciar mudanças nos requisitos

---

## 2. Conceitos Fundamentais

### Requisito vs. Especificação

**Requisito:** *O QUE* o sistema deve fazer

"O sistema deve permitir login de usuários"

**Especificação:** *COMO* o sistema deve fazer

"O login será realizado através de formulário com campos email/senha, validação no servidor via API REST, retorno de token JWT para autenticação"

### Stakeholders

**Definição:** Qualquer pessoa ou grupo que afeta ou é afetado pelo sistema.

**Tipos de Stakeholders:**

- **Primários:** Usuários diretos do sistema
- **Secundários:** Afetados indiretamente (gerentes, suporte técnico)
- **Chave:** Tomadores de decisão (sponsors, product owners)
- **Sombra:** Descobertos durante o projeto (auditores, reguladores)

**Exemplo prático - Sistema de E-commerce:**

- *Primários:* Clientes, vendedores
- *Secundários:* Equipe de marketing, suporte ao cliente
- *Chave:* Gerente de produto, diretor comercial
- *Sombra:* Órgãos reguladores (PROCON), sistemas de pagamento

### Problema vs. Solução

❌ **Foco na Solução (Errado):**

"O sistema precisa ter um botão vermelho no canto direito"

✅ **Foco no Problema (Correto):**

"Os usuários precisam de uma forma rápida de sair do sistema em situações de emergência"

**Por que isso importa?**

- Focar no problema permite múltiplas soluções
- Evita pre-concepções que limitam a criatividade
- Garante que a real necessidade seja atendida

---

## 3. Tipos de Requisitos

### 1. Requisitos Funcionais (RF)

**Definição:** Descrevem *o que* o sistema deve fazer - suas funções e comportamentos.

**Características:**

- Sempre começam com um verbo de ação
- São testáveis e mensuráveis
- Descrevem comportamento observável

**Template:**

O sistema deve [AÇÃO] [OBJETO] [CONDIÇÃO]

**Exemplos:**

- RF01: O sistema deve **autenticar** usuários através de email e senha
- RF02: O sistema deve **gerar** relatório de vendas mensais
- RF03: O sistema deve **enviar** notificação por email quando o estoque estiver baixo
- RF04: O sistema deve **permitir** cancelamento de pedidos em até 24 horas

### 2. Requisitos Não-Funcionais (RNF)

**Definição:** Descrevem *como* o sistema deve funcionar - qualidades e restrições.

**Principais Categorias:**

**Performance**

- **Tempo de resposta:** "Páginas devem carregar em menos de 2 segundos"
- **Throughput:** "Sistema deve suportar 1000 usuários simultâneos"
- **Capacidade:** "Banco deve armazenar 1 milhão de registros"

**Usabilidade**

- **Facilidade de uso:** "Usuário deve conseguir fazer uma compra em menos de 3 cliques"
- **Acessibilidade:** "Interface deve seguir padrões WCAG 2.1"
- **Aprendizagem:** "Novos usuários devem completar primeira tarefa em menos de 5 minutos"

**Confiabilidade**

- **Disponibilidade:** "Sistema deve ter 99.9% de uptime"
- **Recuperação:** "Sistema deve se recuperar de falhas em menos de 30 segundos"
- **Precisão:** "Cálculos financeiros devem ter precisão de 2 casas decimais"

**Segurança**

- **Autenticação:** "Senhas devem ter mínimo 8 caracteres com símbolos"
- **Autorização:** "Apenas administradores podem deletar usuários"
- **Criptografia:** "Dados sensíveis devem ser criptografados em repouso"

**Compatibilidade**

- **Plataformas:** "Deve funcionar em Chrome, Firefox, Safari e Edge"
- **Dispositivos:** "Interface deve ser responsiva para desktop e mobile"
- **Sistemas:** "Deve integrar com sistemas SAP existentes"

**Exemplo Completo - Sistema de Blog: Requisitos Funcionais:**

- RF01: O sistema deve permitir criação de posts
- RF02: O sistema deve permitir comentários em posts
- RF03: O sistema deve permitir busca por título e conteúdo

**Requisitos Não-Funcionais:**

- RNF01: Interface deve carregar em menos de 3 segundos (Performance)
- RNF02: Deve funcionar em dispositivos móveis (Compatibilidade)
- RNF03: Apenas autores podem editar seus próprios posts (Segurança)

### 3. Regras de Negócio

**Definição:** Políticas, regulamentações ou princípios que governam como a organização opera.

**Características:**

- Existem independente do sistema
- Podem mudar ao longo do tempo
- Influenciam múltiplos requisitos

**Exemplos:**

- RN01: Desconto máximo permitido é 20%
- RN02: Pedidos acima de R$ 100 têm frete grátis
- RN03: Usuários menores de 18 anos não podem comprar bebidas alcoólicas

---

## 4. Processo de Análise

### 1. Fases do Processo

**Fase 1: Descoberta**

**Objetivo:** Entender o contexto e identificar stakeholders

**Atividades:**

- Estudar documentos existentes
- Identificar todos os stakeholders
- Entender o domínio do problema
- Definir escopo inicial

**Entregas:**

- Lista de stakeholders
- Glossário de termos do domínio
- Documento de contexto

**Fase 2: Elicitação**

**Objetivo:** Extrair requisitos dos stakeholders

**Atividades:**

- Realizar entrevistas
- Conduzir workshops
- Observar processos atuais
- Analisar sistemas similares

**Entregas:**

- Requisitos brutos (não organizados)
- Atas de reuniões
- Artefatos de processo atual

**Fase 3: Análise**

**Objetivo:** Organizar, priorizar e refinar requisitos

**Atividades:**

- Categorizar requisitos
- Identificar dependências
- Resolver conflitos
- Priorizar requisitos

**Entregas:**

- Lista priorizada de requisitos
- Matriz de dependências
- Log de decisões

**Fase 4: Especificação**

**Objetivo:** Documentar requisitos de forma clara e precisa

**Atividades:**

- Escrever requisitos usando templates
- Criar casos de uso
- Definir critérios de aceitação
- Revisar documentação

**Entregas:**

- Documento de requisitos
- Casos de uso
- Critérios de aceitação

**Fase 5: Validação**

**Objetivo:** Confirmar que requisitos estão corretos

**Atividades:**

- Revisar com stakeholders
- Criar protótipos
- Realizar testes de conceito
- Aprovação formal

**Entregas:**

- Requisitos aprovados
- Protótipos validados
- Atas de aprovação

### 2. Ciclo Iterativo

**Importante:** O processo NÃO é linear. É comum voltar a fases anteriores quando:

- Novos stakeholders são descobertos
- Requisitos conflitantes são identificados
- Mudanças no contexto ocorrem
- Feedback dos usuários é recebido

---

## 5. Técnicas de Levantamento

### 1. Entrevistas

**Quando usar:**

- Início do projeto para entender contexto geral
- Quando precisar de informações detalhadas
- Para esclarecer requisitos conflitantes

**Como conduzir: Preparação:**

- Pesquise sobre o entrevistado e seu papel
- Prepare roteiro com perguntas abertas
- Agende em horário conveniente
- Tenha ferramentas de registro (gravador, notas)

**Durante a entrevista:**

- Comece com perguntas contextuais
- Use técnicas de escuta ativa
- Faça perguntas de esclarecimento
- Evite induzir respostas

**Após a entrevista:**

- Transcreva anotações imediatamente
- Envie resumo para validação
- Identifique pontos que precisam de esclarecimento

**Exemplo de roteiro:**

```
ENTREVISTA - GERENTE DE VENDAS
Objetivo: Entender processo atual de vendas

1. CONTEXTO
   1. Pode me descrever um dia típico seu?
   1. Quais são seus principais desafios?
2. PROCESSO ATUAL
   1. Como vocês fazem vendas hoje?
   1. Que ferramentas utilizam?
   1. Onde vocês sentem mais dificuldade?
3. NECESSIDADES
   1. O que gostaria que fosse diferente?
   1. Se pudesse ter uma varinha mágica, o que mudaria?
4. CRITÉRIOS DE SUCESSO
- Como saberia que o novo sistema está funcionando?
- Que métricas são importantes para você?
```

### 2. Observação

**Quando usar:**

- Para entender processos complexos
- Quando há diferença entre o que dizem e fazem
- Para identificar necessidades não verbalizadas

**Técnicas:**

- **Shadowing:** Acompanhar usuário durante trabalho
- **Fly on the wall:** Observar sem interferir
- **Contextual inquiry:** Observar e fazer perguntas

**Exemplo prático:** Observando um atendente de call center, você pode descobrir que ele usa uma planilha Excel paralela ao sistema oficial porque o sistema é muito lento. Isso revela requisitos de performance que não seriam mencionados em entrevista.

### 3. Workshops

**Quando usar:**

- Para alinhar visões de múltiplos stakeholders
- Para resolver conflitos de requisitos
- Para brainstorming de soluções

**Estrutura típica:**

1.  **Abertura** (15 min): Objetivos e regras
2.  **Contexto** (30 min): Apresentação do problema
3.  **Divergência** (60 min): Levantamento de ideias
4.  **Convergência** (45 min): Organização e priorização
5.  **Fechamento** (15 min): Próximos passos

### 4. Análise de Documentos

**Documentos úteis:**

- Manuais de processo existentes
- Regulamentações e normas
- Sistemas similares
- Reclamações de usuários
- Dados de uso de sistemas atuais

### 5. Questionários

**Quando usar:**

- Para coletar dados de muitas pessoas
- Para validar prioridades
- Para coletar feedback sobre protótipos

**Tipos de perguntas:**

- **Fechadas:** Para dados quantitativos
- **Abertas:** Para insights qualitativos
- **Escala:** Para medir intensidade de opiniões

---

## 6. Documentação de Requisitos

### 1. Características de Bons Requisitos

**Mnemônico: CORRECT**

- **C**ompletos: Toda informação necessária está presente
- **C**orretos: Livres de erros
- **R**astreáveis: Podem ser ligados à origem e implementação
- **R**elevantes: Importantes para o sucesso do projeto
- **E**specíficos: Não ambíguos
- **C**onsistentes: Não conflitam entre si
- **T**estáveis: Podem ser verificados

### 2. Escrevendo Requisitos Funcionais

**Template básico:**

O sistema deve [AÇÃO] [OBJETO] [CONDIÇÃO/RESTRIÇÃO]

**Exemplos bem escritos:**

- ✅ "O sistema deve enviar email de confirmação ao usuário dentro de 5 minutos após o cadastro"
- ✅ "O sistema deve permitir cancelamento de pedidos apenas se o status for 'Aguardando pagamento'"

**Exemplos mal escritos:**

- ❌ "O sistema deve ser rápido" (muito vago)
- ❌ "O sistema deve ter uma tela bonita" (subjetivo)
- ❌ "O sistema deve usar banco MySQL" (solução, não requisito)

### 3. Critérios de Aceitação

**Definição:** Condições específicas que devem ser atendidas para que um requisito seja considerado implementado corretamente.

**Formato Given-When-Then:**

DADO QUE [contexto inicial] QUANDO [ação é executada] ENTÃO [resultado esperado]

**Exemplo:**

RF05: O sistema deve validar CPF do usuário

Critérios de Aceitação:

```
CA01: DADO QUE usuário informa CPF válido
      QUANDO submete formulário de cadastro
      ENTÃO cadastro deve ser aceito

CA02: DADO QUE usuário informa CPF inválido
      QUANDO submete formulário de cadastro
      ENTÃO deve exibir mensagem "CPF inválido"

CA03: DADO QUE usuário não informa CPF
      QUANDO submete formulário de cadastro
      ENTÃO deve exibir mensagem "CPF é obrigatório"
```

### 4. Casos de Uso

**Quando usar:**

- Para descrever interações complexas
- Para mostrar fluxos alternativos
- Para documentar regras de negócio

**Template de Caso de Uso:**

```
CASO DE USO: [Nome]

ATOR: [Quem executa]
OBJETIVO: [O que quer alcançar]

PRÉ-CONDIÇÕES:
- [Condições que devem existir antes]

FLUXO PRINCIPAL:
1. [Passo 1]
2. [Passo 2]
3. [Passo 3]

FLUXOS ALTERNATIVOS:
A1. [Condição alternativa]
  A1.1. [Passo alternativo]
  A1.2. [Retorna ao passo X do fluxo principal]

PÓS-CONDIÇÕES:
- [Estado do sistema após execução]
```

**Exemplo completo:**

```
CASO DE USO: Realizar Login

ATOR: Usuario do sistema

OBJETIVO: Acessar área restrita do sistema

PRÉ-CONDIÇÕES:
- Usuário possui conta ativa no sistema
- Sistema está disponível

FLUXO PRINCIPAL:
1. Usuário acessa página de login
2. Sistema exibe formulário com campos email e senha
3. Usuário preenche email e senha
4. Usuário clica em "Entrar"
5. Sistema valida credenciais
6. Sistema redireciona para página inicial
7. Caso de uso termina

FLUXOS ALTERNATIVOS:
A1. Credenciais inválidas (passo 5)
  A1.1. Sistema exibe mensagem "Email ou senha incorretos"
  A1.2. Retorna ao passo 2

A2. Conta bloqueada (passo 5)
  A2.1. Sistema exibe mensagem "Conta temporariamente bloqueada"
  A2.2. Sistema envia email com instruções de desbloqueio
  A2.3. Caso de uso termina

PÓS-CONDIÇÕES:
- Usuário está autenticado no sistema
- Sessão de usuário está ativa
```

---

## 7. Validação e Verificação

### 1. Diferença entre Validação e Verificação

**Verificação:** "Estamos construindo o produto corretamente?"

- Técnica: Revisões, inspeções, análise
- Foco: Consistência interna dos requisitos

**Validação:** "Estamos construindo o produto correto?"

- Técnica: Protótipos, testes com usuários
- Foco: Adequação às necessidades reais

### 2. Técnicas de Verificação

**Revisão por Pares**

- Outro analista revisa os requisitos
- Foco em clareza, completude e consistência
- Use checklists padronizados

**Inspeção Formal**

- Processo estruturado com papéis definidos
- Moderador, autor, revisores, escriba
- Métricas de defeitos encontrados

**Análise de Dependências**

- Verificar se requisitos conflitam
- Identificar dependências em cascata
- Usar matriz de rastreabilidade

### 3. Técnicas de Validação

**Prototipagem**

**Tipos de protótipos:**

- **Papel:** Sketches e wireframes
- **Digital:** Mockups interativos
- **Funcionais:** Versões simplificadas do sistema

**Vantagens:**

- Feedback visual imediato
- Identifica problemas de usabilidade
- Valida fluxos de processo

**Cenários e Simulações**

- Criar histórias de uso realistas
- Simular situações de exceção
- Testar com dados reais (anonimizados)

**Revisão com Stakeholders**

- Apresentar requisitos em linguagem do negócio
- Usar exemplos concretos
- Documentar feedback e mudanças

---

## 8. Casos Práticos

### 1. Caso: Sistema de Biblioteca

**Contexto:** Uma biblioteca universitária quer digitalizar seu sistema de empréstimos.

**Stakeholders identificados:**

- Bibliotecários (cadastro de livros, controle de empréstimos)
- Estudantes (busca e reserva de livros)
- Professores (reservas prioritárias)
- Administração (relatórios e multas)

**Processo de análise:**

**Passo 1: Entrevistas iniciais**

*Descobertas principais:*

- Sistema atual é manual com fichas
- Muitos livros são perdidos ou devolvidos em atraso
- Estudantes reclamam de não saber se livro está disponível
- Bibliotecários gastam muito tempo com controle manual

**Passo 2: Observação do processo atual**

*Insights:*

- Processo de busca é lento (catalogos físicos)
- Renovações requerem ida física à biblioteca
- Multas são calculadas manualmente e há erros

**Passo 3: Workshop de requisitos**

*Priorizações:*

1.  **MUST:** Catálogo online, controle de empréstimos
2.  **SHOULD:** Reservas online, cálculo automático de multas
3.  **COULD:** Notificações por email, integração com sistema acadêmico
4.  **WON'T:** Compra de livros online (fora do escopo)

**Requisitos resultantes: Funcionais:**

- RF01: Sistema deve permitir busca de livros por título, autor e assunto
- RF02: Sistema deve controlar empréstimos com prazos por tipo de usuário
- RF03: Sistema deve calcular multas automaticamente por atraso
- RF04: Sistema deve permitir renovação online se não há reservas
- RF05: Sistema deve enviar notificações de vencimento por email

**Não-funcionais:**

- RNF01: Busca deve retornar resultados em menos de 3 segundos
- RNF02: Sistema deve estar disponível 99% do tempo durante horário de funcionamento
- RNF03: Interface deve ser acessível para pessoas com deficiência visual
- RNF04: Deve integrar com sistema de autenticação da universidade

**Regras de negócio:**

- RN01: Estudantes podem emprestar até 3 livros por 15 dias
- RN02: Professores podem emprestar até 10 livros por 30 dias
- RN03: Multa é R$ 0,50 por dia de atraso
- RN04: Usuário com multa pendente não pode fazer novos empréstimos

### 2. Caso: App de Delivery

**Contexto:** Restaurante local quer criar app para delivery próprio.

**Stakeholders:**

- Clientes (pedidos online)
- Cozinha (gerenciamento de pedidos)
- Entregadores (otimização de rotas)
- Gerência (relatórios e controle)

**Desafios encontrados:**

- Cliente quer "igual ao iFood" mas com orçamento limitado
- Cozinha tem pouca familiaridade com tecnologia
- Entregadores usam apenas celular
- Gerência quer muitos relatórios diferentes

**Solução de priorização:** Usando técnica MoSCoW com orçamento fixo:

**MUST (70% do orçamento):**

- Catálogo de produtos
- Carrinho de compras
- Processamento de pedidos
- Painel simples para cozinha

**SHOULD (20% do orçamento):**

- App para entregadores
- Notificações push
- Histórico de pedidos

**COULD (10% do orçamento):**

- Programa de fidelidade
- Avaliações de produtos

**WON'T (para versão 1):**

- Integração com redes sociais
- Chatbot de atendimento
- Relatórios avançados

---

## 9. Erros Comuns e Como Evitar

### 1. Erros de Processo

**Erro: Pular a fase de descoberta**

**Sintomas:**

- Requisitos surgem durante desenvolvimento
- Stakeholders importantes são esquecidos
- Escopo cresce descontroladamente

**Como evitar:**

- Sempre invista tempo em entender o contexto
- Mapeie stakeholders de forma sistemática
- Documente premissas e restrições

**Erro: Não validar requisitos**

**Sintomas:**

- Sistema pronto não atende necessidades
- Usuários rejeitam a solução
- Muito retrabalho após deploy

**Como evitar:**

- Crie protótipos cedo
- Valide requisitos com múltiplos stakeholders
- Use técnicas de feedback iterativo

### 2. Erros de Documentação

**Erro: Requisitos ambíguos**

**Exemplos problemáticos:**

- ❌ "Sistema deve ser rápido"
- ❌ "Interface deve ser intuitiva"
- ❌ "Deve funcionar bem"

**Como evitar:**

- Use critérios mensuráveis
- Defina claramente termos vagos
- Inclua exemplos concretos

**Erro: Misturar requisitos com soluções**

**Exemplo problemático:**

- ❌ "Sistema deve usar dropdown para seleção de país"

**Correção:**

- ✅ "Sistema deve permitir seleção de país de forma rápida e precisa"

### 3. Erros de Comunicação

**Erro: Usar jargão técnico com usuários**

**Como evitar:**

- Adapte linguagem ao público
- Use exemplos do dia-a-dia
- Confirme entendimento constantemente

**Erro: Não gerenciar expectativas**

**Como evitar:**

- Seja transparente sobre limitações
- Explique trade-offs de decisões
- Documente mudanças e impactos

---

## 10. Templates e Ferramentas

### 1. Template de Documento de Requisitos

```markdown
# DOCUMENTO DE REQUISITOS
**Projeto:** [Nome do Projeto]
**Versão:** [Versão]
**Data:** [Data]
**Autor:** [Nome do Analista]

## 1. INTRODUÇÃO

### 1.1 Objetivo do Documento
### 1.2 Escopo do Projeto
### 1.3 Stakeholders
### 1.4 Glossário

## 2. VISÃO GERAL

### 2.1 Contexto do Negócio
### 2.2 Problema Atual
### 2.3 Solução Proposta
### 2.4 Benefícios Esperados

## 3. REQUISITOS FUNCIONAIS

[Para cada requisito:]

- **ID:** RF001
- **Título:** [Nome descritivo]
- **Descrição:** [Descrição detalhada]
- **Prioridade:** [Alta/Média/Baixa]
- **Critérios de Aceitação:**
  - CA001: [Critério 1]
  - CA002: [Critério 2]

## 4. REQUISITOS NÃO-FUNCIONAIS

[Organizado por categoria:]

### 4.1 Performance
### 4.2 Usabilidade
### 4.3 Segurança
### 4.4 Compatibilidade

## 5. REGRAS DE NEGÓCIO

[Lista numerada com regras claras]

## 6. RESTRIÇÕES E LIMITAÇÕES

### 6.1 Técnicas
### 6.2 Orçamentárias
### 6.3 Temporais

## 7. CASOS DE USO

[Casos de uso detalhados para fluxos principais]

## 8. PROTÓTIPOS E MOCKUPS

[Links ou anexos com protótipos]

## 9. CRITÉRIOS DE ACEITAÇÃO DO PROJETO

[Como saber que o projeto foi bem-sucedido]

## 10. RISCOS E MITIGAÇÕES

[Principais riscos identificados e planos de mitigação]

## 11. APROVAÇÕES

[Seção para assinaturas dos stakeholders]
```

### 2. Checklist de Qualidade

**Antes de finalizar um requisito, verifique: Clareza**

- Usa linguagem clara e objetiva?
- Evita jargões desnecessários?
- Pode ser entendido por não-técnicos?

**Completude**

- Todas as informações necessárias estão presentes?
- Condições de exceção foram consideradas?
- Dependências estão documentadas?

**Consistência**

- Não conflita com outros requisitos?
- Usa terminologia consistente?
- Segue padrões do documento?

**Testabilidade**

- É possível verificar se foi implementado?
- Critérios de aceitação são mensuráveis?
- Comportamento esperado está claro?

**Viabilidade**

- É tecnicamente possível?
- Está dentro das restrições do projeto?
- Tempo/custo são razoáveis?

### 3. Ferramentas Recomendadas

**Para Documentação**

- **Confluence:** Documentação colaborativa
- **Notion:** Organização flexível de requisitos
- **GitBook:** Documentação técnica estruturada
- **Google Docs:** Colaboração simples e feedback

**Para Prototipagem**

- **Figma:** Design de interfaces e protótipos interativos
- **InVision:** Protótipos clicáveis
- **Balsamiq:** Wireframes rápidos
- **Draw.io:** Diagramas e fluxos

**Para Gerenciamento**

- **Jira:** Rastreamento de requisitos e issues
- **Trello:** Organização visual simples
- **Azure DevOps:** Gerenciamento completo de projeto
- **Monday.com:** Gestão de projetos e requisitos

**Para Colaboração**

- **Miro:** Workshops virtuais e brainstorming
- **Slack:** Comunicação da equipe
- **Zoom:** Entrevistas e reuniões
- **Loom:** Gravação de explicações e demos

---

## Conclusão

A análise de requisitos é uma disciplina que combina **habilidades técnicas** (documentação, modelagem) com **habilidades interpessoais** (comunicação, negociação). O sucesso nesta área vem da prática constante e da capacidade de se adaptar a diferentes contextos e stakeholders.

**Lembre-se sempre:**

- **Foque no problema, não na solução**
- **Comunique-se na linguagem do seu público**
- **Valide constantemente seu entendimento**
- **Documente decisões e suas justificativas**
- **Seja iterativo e flexível**

A qualidade dos requisitos determina diretamente o sucesso do projeto. Invista tempo nesta fase - é o melhor ROI que você pode ter em desenvolvimento de software.

*Esta apostila serve como guia de referência durante as atividades práticas de análise de requisitos. Consulte as seções relevantes conforme necessite durante a execução dos projetos.*