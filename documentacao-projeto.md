# Documentando o Projeto Final: The Machine

**Estudante:** André Lumertz Martins
**Curso:** Análise e Desenvolvimento de Sistemas (ADS)

---

## Resumo do Projeto

O gerenciamento operacional de cafeterias de cafés especiais frequentemente enfrenta gargalos na rápida atualização de cardápios e na disseminação de conhecimento técnico sobre grãos e métodos aos clientes finais. A falta de centralização e a alta rotatividade de equipes dificultam o atendimento especializado e o controle gerencial unificado.

Este trabalho apresenta o **The Machine**, um ecossistema digital integrado que une um sistema de gestão administrativa tradicional em PHP 8.2 a um motor de Inteligência Artificial Generativa em Python (FastAPI) baseado em **RAG (Retrieval-Augmented Generation)**. A implementação desta arquitetura conteinerizada otimiza os recursos computacionais e reduz a latência de inferência, viabilizando o controle de estoque de produtos (CRUD) concomitante a um atendimento automatizado e altamente especializado ao cliente.

---

## Definição do Problema

O mercado de cafés especiais em Porto Alegre/RS, especificamente contextualizado no cenário do **Blackout Café**, demanda uma operação administrativa ágil combinada a uma forte vertente educativa para o consumidor. No modelo de negócios tradicional, processos cruciais como o controle de estoque, a emissão de relatórios gerenciais estruturados e o suporte técnico sobre harmonizações e métodos de extração operam de forma fragmentada ou não informatizada.

A etapa de *discovery* do projeto identificou três problemas principais:

1. **Gargalo de Atendimento Técnico:** Clientes demandam informações detalhadas sobre a origem dos grãos e perfis sensoriais, sobrecarregando os atendentes.
2. **Fragmentação de Ferramentas:** A separação física entre o sistema de vendas/estoque e a base de conhecimento institucional gera inconsistência de dados.
3. **Escalabilidade e Custo de Infraestrutura:** A integração de modelos modernos de Inteligência Artificial (Deep Learning) geralmente exige infraestruturas locais pesadas e caras, inviáveis para pequenas e médias empresas do setor alimentício.

### Benchmarking Comparativo

Para contextualizar a relevância da solução proposta, a tabela abaixo estabelece um benchmarking entre os sistemas comerciais convencionais e as inovações introduzidas pelo The Machine:

| Características / Funcionalidades | Sistemas de PDV Tradicionais | Chatbots de Atendimento Genéricos | The Machine (Solução Proposta) |
|---|---|---|---|
| Gerenciamento de Produtos (CRUD) | Sim | Não | Sim |
| Geração de Relatórios Gerenciais | Sim (Estático) | Não | Sim (PDF Dinâmico via Dompdf) |
| Base de Conhecimento Dinâmica | Não | Parcial (Regras Fixas) | Sim (RAG via LlamaIndex) |
| Filtro de Segurança Furtivo | Não aplicável | Não | Sim (Controle de Escopo) |
| Infraestrutura Unificada | Local ou Nuvem | Nuvem Separada | Container Híbrido Unificado (Render) |

---

## Objetivos

### Objetivo Geral

Desenvolver e implantar um ecossistema digital integrado (PHP + Python) capaz de centralizar a gestão administrativa de produtos e automatizar o atendimento especializado ao cliente através de uma arquitetura RAG para o Blackout Café.

### Objetivos Específicos

- Implementar um painel administrativo seguro em PHP 8.2 para o controle completo (CRUD) de itens do cardápio.
- Desenvolver um gerador de relatórios gerenciais em tempo real, garantindo que qualquer inclusão ou alteração de produto no CRUD seja refletida instantaneamente no documento compilado.
- Construir um microserviço em Python (FastAPI) integrado ao LlamaIndex para indexar e processar o cardápio técnico em PDF através de embeddings reais.
- Criar uma camada de proteção (**Filtro de Intenções Furtivo**) para mitigar desvios de escopo e redirecionar transações para canais humanos.
- Orquestrar e implantar a aplicação em um ambiente conteinerizado único no Render, utilizando o banco de dados distribuído TiDB Cloud em conformidade com o protocolo MySQL.

---

## Stack Tecnológico

O desenvolvimento do ecossistema é baseado em componentes de mercado consolidados, documentados e estruturados para alta performance:

| Tecnologia | Papel no Projeto |
|---|---|
| **PHP 8.2 & MySQL** | Núcleo administrativo do sistema (core). Robustez na manipulação de sessões de segurança, tipagem limpa de dados e natividade com servidores Apache. |
| **TiDB Cloud** | Banco de dados de produção com infraestrutura elástica e distribuída na nuvem, mantendo compatibilidade 100% nativa com o protocolo MySQL (conectividade via PDO sem alteração de código). |
| **FastAPI & Uvicorn (Python 3)** | Runtimes assíncronos de alta performance para expor a API do Chatbot de IA na porta 8001, garantindo inferência com tempo de resposta na escala de milissegundos. |
| **LlamaIndex Core** | Framework de orquestração para a arquitetura RAG. Estrutura os índices vetoriais a partir do arquivo contido no diretório `documents/`. |
| **Hugging Face (Embeddings)** | Provedor dos modelos de representação vetorial para converter o texto do cardápio técnico em formato de alta dimensionalidade, permitindo busca semântica local. |
| **Groq LPU (Llama 3.3 70B)** | Infraestrutura de processamento em nuvem baseada em Unidades de Processamento de Linguagem (LPUs), reduzindo a latência cognitiva a quase zero via API Key. |
| **Tailwind CSS** | Framework utilitário de CSS para interface fluida, responsiva e com identidade visual minimalista em dark mode. |
| **Composer & Dompdf** | Ferramentas de gerenciamento e compilação de dependências no PHP para renderização de HTML para PDF em nível de servidor. |
| **Docker** | Tecnologia de virtualização a nível de sistema operacional para fundir os runtimes do Apache/PHP e do Python em uma única receita unificada de deploy. |

---

## Descrição da Solução

O sistema funciona de maneira unificada através de um fluxo assíncrono entre duas linguagens. A interface pública do usuário exibe o cardápio institucional responsivo e a caixa de diálogo do chatbot especialista.

### Fluxo do Chatbot (IA)

Quando um cliente interage com a IA, o front-end em JavaScript dispara uma requisição para uma ponte interna (`chat-bridge.php`), que encaminha a mensagem ao microsserviço Python na porta `8001` (ou na rede interna do Render em produção). Dentro do motor Python, a mensagem passa obrigatoriamente pelo **Filtro de Intenções Furtivo**, implementado em duas camadas complementares antes de qualquer chamada ao LLM:

**Camada 1 — Interceptação Determinística (zero custo de inferência):**
A função `contem_intencao_pedido()` verifica a mensagem contra uma lista de expressões de compra e pagamento (`EXPRESSOES_PEDIDO`). Se houver correspondência, o sistema retorna imediatamente uma mensagem de redirecionamento para os canais humanos (WhatsApp/e-mail), sem consultar o RAG nem o modelo de linguagem.

**Camada 2 — Contenção Semântica via System Prompt:**
Para mensagens que passam pela primeira camada, o `system_prompt` injetado no Llama 3.3 instrui o modelo a responder exclusivamente com base nos documentos indexados, nunca inventar preços ou produtos, e tratar perguntas fora do domínio do café de forma breve e educada.

O fluxo completo para mensagens dentro do escopo segue:

1. A mensagem passa pela Camada 1 sem correspondência na lista de intenções;
2. O motor consulta o índice vetorial gerado a partir do PDF técnico (`blackout_cafes_especiais.pdf`), processado pelo modelo de embeddings `BAAI/bge-small-en-v1.5` via Hugging Face;
3. O contexto semântico relevante é extraído e anexado à requisição;
4. Os dados consolidados são enviados ao modelo Llama 3.3 na Groq LPU, contido pela Camada 2;
5. A resposta tratada é devolvida ao usuário.

### Fluxo Administrativo

Na camada administrativa restrita (`login.php`), o gestor autenticado acessa o Dashboard (`admin.php`), de onde pode executar operações de inserção, edição ou remoção de produtos. Ao acionar a exportação de relatórios, o script `gerador-pdf.php`:

1. Faz uma consulta dinâmica ao banco de dados (MySQL local via porta `3307` ou TiDB Cloud em produção via porta `4000`);
2. Coleta instantaneamente as informações mais recentes do sistema;
3. Monta a estrutura visual via `conteudo-pdf.php`;
4. Aciona a biblioteca Dompdf, garantindo que novos cafés cadastrados apareçam imediatamente no documento gerado para download.

---

## Arquitetura

Os artefatos gerados, histórico de revisões semânticas, controle de branches corporativas (`dev`, `main` e correções de infraestrutura) e o código-fonte completo estão centralizados e públicos no repositório oficial do projeto:

> **Repositório Oficial:** [https://github.com/andrelumertz/the-machine](https://github.com/andrelumertz/the-machine)

### Artefatos de Engenharia Realizados

- **Benchmarking Comparativo:** Análise de concorrência e mapeamento de diferenciais de mercado.
- **Modelo de Entidades e Relacionamentos (Diagrama ER):** Mapeamento do banco de dados `blackout_cafe` para controle de produtos.
- **Protótipos de Interface:** Estruturação visual do painel administrativo e do Chatbot.


---

## Validação

### Estratégia

A validação da eficácia do ecossistema The Machine baseou-se em testes de carga de infraestrutura e usabilidade assistida com stakeholders reais do estabelecimento. Foram realizadas:

- **Simulações locais** (via XAMPP em ambiente Windows) para validar o comportamento da persistência offline;
- **Testes estruturados no servidor de produção** do Render para monitorar a resposta do TiDB Cloud em cenários de requisições simultâneas ao gerador de PDF;
- **Avaliação de alucinação e controle de escopo** para verificar o comportamento do Filtro de Intenções Furtivo em duas frentes: (a) a interceptação determinística via `EXPRESSOES_PEDIDO` para intenções de compra e pagamento, e (b) a contenção semântica do `system_prompt` para perguntas fora do domínio do café, prevenindo respostas inventadas pelo modelo.

### Consolidação dos Dados Coletados

| Métrica | Resultado |
|---|---|
| Tempo médio de compilação e download do relatório PDF (produção) | < **1.2 segundos** |
| Latência média das respostas geradas pela Groq LPU (RAG) | < **850 milissegundos** |

Os testes empíricos validaram o modelo conteinerizado híbrido como uma solução comercialmente viável, barata e estável para a operação real, atestando o acerto na otimização das chamadas de banco de dados e a eficiência da indexação semântica via Hugging Face com vetores mantidos em memória RAM.

---

## Conclusões

O projeto atingiu plenamente os objetivos propostos ao demonstrar que restrições orçamentárias de hospedagem em nuvem podem ser superadas com engenharia de software e arquitetura inteligente. O problema da fragmentação operacional do estabelecimento foi mitigado com a entrega de uma aplicação fluida, segura e de baixíssimo custo de manutenção.

### Limitações do Projeto

Como limitação atual, o contêiner depende de conexões síncronas de API para o processamento de linguagem natural.

### Perspectivas Futuras

Como trabalho futuro, planeja-se a evolução do sistema para suportar a **atualização automática da base de conhecimento do RAG em tempo real** a partir de alterações feitas diretamente no CRUD do banco de dados (**Event-Driven RAG**), eliminando a necessidade de substituição manual do PDF técnico do cardápio.

---

## Referências Bibliográficas

- GROQ. *Groq LPU Inference Engine and Llama 3.3 Integration API Documentation*. Disponível em: https://console.groq.com/docs. Acesso em: 2026.

- HUGGING FACE. *Hugging Face Transformers and Embeddings Documentation*. Disponível em: https://huggingface.co/docs. Acesso em: 2026.

- LLAMAINDEX. *LlamaIndex Documentation: Data Connectors and Vector Store Indexes*. Disponível em: https://docs.llamaindex.ai. Acesso em: 2026.

- THE PHP FOUNDATION. *PHP 8.2 Reference Manual: PDO MySQL and Type System Enhancements*. Disponível em: https://www.php.net. Acesso em: 2026.

- TIDB CLOUD. *TiDB Serverless: MySQL-Compatible Distributed Database Documentation*. Disponível em: https://docs.pingcap.com/tidb-in-the-cloud. Acesso em: 2026.
