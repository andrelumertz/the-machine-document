# Documentação do Projeto: The Machine — Blackout Café

**Curso:** Análise e Desenvolvimento de Sistemas (ADS)  
**Autor:** André Lumertz Martins  

## 1. Resumo do Projeto
O setor de cafeterias especiais enfrenta dificuldades em escalar o atendimento personalizado e a gestão de estoques técnicos sem perder a agilidade operacional. O **The Machine** é um ecossistema digital que integra um sistema de gestão a um assistente virtual inteligente. Atualmente, o mercado carece de sistemas acessíveis que unam dados transacionais à inteligência artificial de recuperação. Esta solução permite que a gestão administrativa e o suporte técnico ao cliente coexistam em uma plataforma única. Como consequência, espera-se uma redução no tempo de resposta ao cliente e maior precisão no controle de insumos.

## 2. Definição do Problema
O problema central reside na fragmentação da informação em pequenas empresas do setor alimentício. Segundo documentações técnicas de gestão de processos, a falta de informatização em cafeterias leva a erros de estoque e subutilização de manuais técnicos de grãos. 
* **Discovery:** Observou-se que atendentes muitas vezes desconhecem as características sensoriais de cafés específicos, e gestores utilizam métodos manuais para controle de produtos. 
* **Benchmarking:** Sistemas tradicionais focam apenas em vendas (PDV). O diferencial do *The Machine* é a arquitetura RAG, que automatiza a consulta de manuais técnicos em PDF, algo inexistente em soluções populares de baixo custo.

## 3. Objetivos
* **Objetivo Geral:** Desenvolver um sistema integrado de gestão e suporte inteligente para cafeterias especializadas.
* **Objetivos Específicos:**
    1.  Implementar um CRUD robusto para controle de produtos e inventário.
    2.  Desenvolver um módulo de Inteligência Artificial baseado em RAG para consulta de documentos técnicos.
    3.  Automatizar a geração de relatórios gerenciais em formato PDF.
    4.  Garantir a segurança dos dados e o deploy contínuo em ambiente de nuvem.


## 4. Stack Tecnológico
* **PHP:** Linguagem servidor para a lógica de negócio devido à sua maturidade e suporte a drivers de banco de dados (PDO).
* **Python:** Utilizado para o motor de IA, por ser a linguagem padrão para LlamaIndex e processamento de linguagem natural.
* **MySQL:** Sistema Gerenciador de Banco de Dados (SGBD) relacional para garantir a integridade referencial dos dados transacionais.
* **Groq LPU (Language Processing Unit):** Infraestrutura de inferência de altíssima performance utilizada para executar o LLM (Large Language Model) com latência próxima de zero, garantindo respostas em tempo real.
* **Hugging Face:** Ecossistema utilizado para o provisionamento de modelos de *embeddings* e bibliotecas de NLP, servindo como base para o processamento e compreensão semântica dos documentos.
* **LlamaIndex:** Framework de dados utilizado para a ingestão, indexação e conexão do conhecimento especializado (PDFs) aos modelos de linguagem.
* **ChromaDB:** Banco de dados vetorial (Vector Database) para o armazenamento e recuperação eficiente de informações através de busca semântica.

## 5. Descrição da Solução
A solução é dividida em dois núcleos complementares, com escopos bem definidos para garantir a eficiência operacional:

* **Núcleo de Gestão Administrativa (PHP/MySQL):** Responsável pelo controle de inventário, manutenção do catálogo de produtos e geração de relatórios. Este módulo gerencia os dados estruturados necessários para a operação da cafeteria.
* **Núcleo de Inteligência Consultiva (IA/RAG):** O chatbot atua exclusivamente como um **especialista técnico em cafés**. Ele não realiza transações financeiras ou finalização de vendas; seu objetivo é fornecer suporte informativo de alta precisão. 

Através da arquitetura RAG com **LlamaIndex** e **ChromaDB**, o assistente realiza buscas semânticas em manuais e documentos técnicos. Isso permite que ele responda dúvidas sobre métodos de preparo, características sensoriais dos grãos e compatibilidade de blends, auxiliando o cliente na tomada de decisão e enriquecendo a experiência de consumo sem a necessidade de intervenção imediata de um barista.

## 9. Referências Bibliográficas
* **PHP Documentation.** Official Manual. Disponível em: https://www.php.net/manual/pt_BR/.
* **LlamaIndex Documentation.** Data Framework for LLM Applications. Disponível em: https://docs.llamaindex.ai/.
* **ChromaDB.** Open Source Vector Database. Disponível em: https://docs.trychroma.com/.