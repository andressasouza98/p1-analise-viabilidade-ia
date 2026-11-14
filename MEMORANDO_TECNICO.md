# MEMORANDO TÉCNICO — Análise de Viabilidade de IA no Atendimento Estudantil

**Para:** Direção da Instituição de Ensino Digital  
**De:** Andressa Alves de Souza (Analista Técnica de IA)  
**Projeto:** P1 — Análise de viabilidade para um caso real  

---

## 1. Resumo Executivo

*(A ser consolidado após a conclusão das análises e testes nos níveis Básico, Intermediário e Avançado).*

---

## 2. Análise das Funcionalidades

### 2.1 Decomposição (Fatoração) do Fluxo de Atendimento

Para evitar o erro arquitetural de avaliar o atendimento como um bloco monolítico ("aprovar ou rejeitar tudo"), o processo atual da instituição de ensino digital foi fatiado em atividades menores e independentes, classificadas segundo sua natureza operacional predominante:

| Atividade do Fluxo | Natureza Predominante | Entrada | Saída Esperada |
| :--- | :--- | :--- | :--- |
| **1. Classificação temática de mensagens livres** | Interpretação de linguagem natural | Texto livre enviado pelo estudante (muitas vezes ambíguo, informal ou com múltiplos assuntos) | Rótulo de categoria temática (ex.: matrícula, documentos, pagamentos, calendário acadêmico, notas, acesso ao ambiente virtual, administrativo) |
| **2. Identificação de prioridade / urgência** | Interpretação de linguagem + Aplicação de regra | Texto da mensagem e metadados de prazo (quando informados) | Nível de prioridade (ex.: baixa, média, alta) e sinalização de urgência |
| **3. Encaminhamento ao setor responsável** | Aplicação de regra determinística | Categoria temática já classificada | Roteamento da solicitação para a fila do setor competente |
| **4. Consulta a informações objetivas e status** | Consulta a informação + Aplicação de regra | Identificador do estudante ou seleção de dúvida factual (ex.: datas do calendário acadêmico, status de pagamento, emissão de documento) | Dado exato extraído da base institucional ou link/procedimento padrão |
| **5. Elaboração de rascunho de resposta** | Geração de texto + Consulta a informação | Mensagem do estudante + base documental/regras institucionais aplicáveis ao tema | Texto preliminar estruturado para auxiliar o atendente na resposta ao estudante |
| **6. Envio automático de respostas simples** | Geração de texto + Decisão operacional | Mensagem classificada como dúvida simples + texto gerado pelo modelo | Resposta enviada diretamente ao estudante sem intermediação humana |
| **7. Análise de revisão de nota, benefício financeiro ou medida acadêmica** | Decisão de alto impacto + Aplicação de regra regulatória | Solicitação do estudante, justificativa, histórico acadêmico/financeiro e normas institucionais | Parecer formal de deferimento, indeferimento ou aplicação de medida administrativa |

---

### 2.2 Análise Aprofundada das 4 Funcionalidades Selecionadas

Foram selecionadas **quatro funcionalidades** representativas do espectro arquitetural da instituição: (1) interpretação semântica pura de baixo impacto; (2) geração de rascunhos assistida; (3) atendimento a consultas factuais e padronizadas; e (4) decisões críticas com impacto direto sobre direitos e finanças do estudante.

---

#### Funcionalidade 1: Classificação Temática de Mensagens em Linguagem Livre

*(Candidata aparentemente adequada a LLM)

* **Qual é a entrada?**
  Mensagens em texto não estruturado enviadas pelos estudantes por canais abertos, frequentemente redigidas com linguagem informal, erros de digitação, tom emocional, lacunas de contexto ou mistura de temas (ex.: dúvida sobre acesso ao ambiente virtual combinada com prazo de entrega).
* **Qual resultado é esperado?**
  A atribuição de uma categoria temática padronizada (matrícula, documentos, pagamentos, calendário acadêmico, revisão de notas, acesso ao ambiente virtual ou solicitações administrativas) e a sinalização quando houver múltiplos assuntos ou ambiguidade.
* **Um LLM é adequado para essa tarefa?**
  **Parcialmente adequado (com restrições).** Diferente de códigos convencionais que exigem entradas estruturadas e falham diante de sinônimos ou redação livre, um LLM possui capacidade nativa de interpretação semântica para mapear intenções em linguagem natural sem exigir treinamento supervisionado prévio (*cold start*). Contudo, por operar de forma probabilística e não determinística, exige saída estruturada rígida e regras explícitas para lidar com mensagens ambíguas.
* **Que erros o modelo poderia cometer?**
  * Classificar incorretamente uma mensagem com múltiplos assuntos (escolhendo apenas um tema secundário e ignorando uma urgência crítica);
  * Inventar categorias fora da taxonomia oficial da instituição;
  * Sofrer desvio de comportamento diante de tentativas de manipulação textual (*prompt injection*) inseridas na mensagem do aluno.
* **Qual seria o impacto desses erros?**
  **Impacto operacional baixo a moderado (reversível).** Um erro de classificação não altera registros acadêmicos nem gera perda financeira direta ao aluno; o efeito principal é o atraso na triagem e o retrabalho da equipe humana para reencaminhar o chamado à fila correta.
* **Existe alternativa mais simples?**
  **Sim.** A adoção de um formulário estruturado ou menu de autoatendimento no portal do estudante, no qual o próprio aluno seleciona o assunto antes de digitar a mensagem, combinado com regras de roteamento por palavras-chave para casos diretos.
* **Seria necessária revisão humana?**
  **Sim, por exceção (triagem híbrida).** Mensagens classificadas com alta clareza em temas triviais podem seguir fluxo automatizado de roteamento, mas mensagens identificadas como ambíguas, com múltiplos assuntos ou com baixa confiança devem ser encaminhadas para triagem humana.

---

#### Funcionalidade 2: Produção de Rascunho de Resposta para Atendimento

*(Candidata a LLM como copiloto com supervisão humana)

* **Qual é a entrada?**
  A mensagem original do estudante (devidamente sanitizada de dados pessoais sensíveis) acompanhada dos trechos oficiais das normas, prazos e procedimentos da instituição pertinentes ao assunto.
* **Qual resultado é esperado?**
  Um rascunho de resposta claro, coeso e ancorado estritamente nas regras institucionais fornecidas no contexto, pronto para ser conferido, ajustado (se necessário) e enviado pelo atendente humano.
* **Um LLM é adequado para essa tarefa?**
  **Adequado apenas como ferramenta de apoio (copiloto), nunca para envio autônomo.** LLMs são otimizados para síntese e fluidez textual em linguagem natural, o que reduz o tempo de redação da equipe. Porém, devido ao **viés da fluência sintática** (capacidade de escrever textos convincentes mesmo quando contêm dados falsos) e à natureza probabilística da geração token a token, o modelo não garante veracidade factual por conta própria.
* **Que erros o modelo poderia cometer?**
  * **Alucinação fática:** inventar prazos, links, nomes de formulários ou exceções administrativas não previstas no contexto;
  * **Omissão de requisitos críticos:** deixar de mencionar um documento obrigatório ou uma condição de prazo;
  * Assumir compromissos institucionais indevidos em tom de autoridade.
* **Qual seria o impacto desses erros?**
  **Impacto alto se enviado diretamente ao aluno; impacto baixo se restrito a rascunho interno.** Se enviado sem revisão, uma instrução ou prazo inventado pode induzir o estudante ao erro, causar perda de prazos acadêmicos, gerar chamados indevidos e criar passivos jurídicos ou danos reputacionais à instituição.
* **Existe alternativa mais simples?**
  **Sim.** Para dúvidas recorrentes e padronizadas, o uso de modelos de resposta prontos (*templates* fixos) vinculados a uma base de Perguntas Frequentes (FAQ) e preenchidos por variáveis determinísticas do sistema.
* **Seria necessária revisão humana?**
  **Sim, obrigatória e síncrona (*Human-in-the-Loop*).** Nenhum rascunho gerado pelo modelo deve ser disparado diretamente ao estudante; o atendente humano atua como validador obrigatório antes do envio final.

---

#### Funcionalidade 3: Consulta a Dados Cadastrais, Status de Pagamento e Calendário Acadêmico

*(Resolvível de forma mais simples e segura por código determinístico)

* **Qual é a entrada?**
  Solicitação objetiva do estudante para consultar informações factuais e estruturadas — como datas oficiais do calendário acadêmico, situação de pagamento/boleto ou emissão de comprovante de matrícula.
* **Qual resultado é esperado?**
  A entrega exata, imediata e auditável do dado oficial registrado no banco de dados institucional (ex.: data limite exata ou situação financeira atualizada), sem margem para interpretação ou variação textual.
* **Um LLM é adequado para essa tarefa?**
  **Não é adequado.** Trata-se de um problema puramente determinístico (consulta exata a regras e registros estruturados). Utilizar um modelo probabilístico gerador de texto para responder fatos exatos introduz latência desnecessária (segundos de inferência em vez de milissegundos), custo linear/exponencial por token e risco grave de inventar valores ou datas.
* **Que erros o modelo poderia cometer?**
  * Fabricar datas de provas, prazos de matrícula ou valores de parcelas incorretos (*alucinação fática*);
  * Confundir regras de períodos letivos distintos;
  * Expor dados de registros internos de forma inadequada caso ocorra falha no isolamento de contexto.
* **Qual seria o impacto desses erros?**
  **Impacto financeiro, operacional e jurídico alto.** Informar um prazo acadêmico errado ou um status de pagamento inexistente prejudica diretamente o planejamento do estudante, gera contestações formais e compromete a confiabilidade operacional da instituição.
* **Existe alternativa mais simples?**
  **Sim, muito superior.** Consultas diretas via SQL/API ao banco de dados relacional institucional integradas a um portal de autoatendimento (com formulários estruturados e validação de entrada) e páginas estáticas de Calendário Acadêmico e FAQ. Essa arquitetura opera em menos de 1 milissegundo, com custo marginal zero por requisição e 100% de rastreabilidade.
* **Seria necessária revisão humana?**
  **Não (na solução determinística recomendada).** Como a consulta direta ao banco de dados por código tradicional é determinística e auditável, o próprio estudante acessa a informação oficial sem necessidade de intermediação humana ou de IA.

---

#### Funcionalidade 4: Apoio à Decisão sobre Revisão de Nota, Benefício Financeiro ou Medida Acadêmica

*(Decisão crítica de alto impacto — Não delegável à IA)

* **Qual é a entrada?**
  Pedidos formais e sensíveis dos estudantes solicitando revisão de avaliação, concessão de bolsas/descontos financeiros ou contestação de sanções/medidas acadêmicas, contendo justificativas textuais, documentos anexos e histórico acadêmico/financeiro.
* **Qual resultado é esperado?**
  Uma decisão administrativa ou pedagógica fundamentada, justa, auditável e alinhada ao regimento interno e à legislação aplicável, deferindo ou indeferindo o pedido com justificativa explícita.
* **Um LLM é adequado para essa tarefa?**
  **Inadequado (contra-indicado).** Decisões que afetam direitos, oportunidades acadêmicas ou a situação financeira dos estudantes exigem determinismo regulatório, isonomia e explicabilidade total (Art. 20 da LGPD). LLMs funcionam como "caixas-pretas" probabilísticas baseadas em pesos vetoriais: eles não aplicam normas jurídicas com rigor lógico, não possuem responsabilidade legal e não conseguem justificar tecnicamente por que um estudante recebeu um benefício e outro não.
* **Que erros o modelo poderia cometer?**
  * **Tratamento enviesado ou discriminatório:** decisões inconsistentes influenciadas pelo estilo de escrita ou persuasão emocional do estudante;
  * **Concessão indevida de benefícios:** ser induzido por argumentos persuasivos ou *prompt injection* a aprovar descontos ou alterações de nota fora das regras;
  * **Falta de explicabilidade e alucinação normativa:** inventar critérios inexistentes para negar um direito legítimo do aluno.
* **Qual seria o impacto desses erros?**
  **Impacto crítico (legal, financeiro, ético e reputacional).** Uma decisão errada altera o histórico escolar do aluno, gera prejuízo financeiro direto à instituição (concessão indevida de bolsas), viola o direito de revisão e explicação previsto na LGPD e expõe a instituição a processos judiciais e sanções regulatórias.
* **Existe alternativa mais simples?**
  **Sim.** Fluxo de análise exclusivamente humana apoiado por regras de elegibilidade determinísticas no sistema acadêmico/financeiro (ex.: verificação automática de pré-requisitos objetivos no banco de dados relacional), mantendo registro formal de parecer e trilha de auditoria.
* **Seria necessária revisão humana?**
  **Sim, a decisão deve ser 100% humana.** Conforme as diretrizes de segurança e governança do projeto, tarefas de alto impacto sobre direitos, notas e finanças não devem ser delegadas a um LLM nem sequer para decisão preliminar automatizada.

---

## 3. Funcionamento e Limitações dos LLMs

*(Relação entre características técnicas dos LLMs e evidências observadas nos testes).*

---

## 4. Prompts Protótipos

*(Evolução versionada dos prompts: objetivo, estrutura, alteração isolada entre versões, resultado observado e limitação remanescente).*

---

## 5. Critérios e Casos de Teste

*(Critérios objetivos definidos pré-teste e matriz completa de resultados com casos fictícios).*

---

## 6. Riscos e Mitigações

*(Análise de riscos éticos, legais/LGPD e operacionais com controles técnicos concretos e responsabilidade humana).*

---

## 7. Recomendação por Funcionalidade

*(Classificação final: Usar IA | Usar IA com revisão humana | Usar solução não baseada em IA | Não implementar nas condições atuais).*

---

## 8. Recomendação sobre Piloto

*(Desenho do piloto controlado de baixo risco com métricas e circuit breakers, ou justificativa para não realização).*

---

## 9. Evolução por Nível

* **Nível Básico (`p1-basico`):** Em construção — Concluída a decomposição do sistema e análise das 4 funcionalidades (`BAS-01`).
* **Nível Intermediário (`p1-intermediario`):** Pendente.
* **Nível Avançado (`p1-avancado`):** Pendente.

---

## 10. Uso de IA no Desenvolvimento

| Ferramenta / Modelo | Tarefa Delegada | Resultado Aproveitado | Correções / Adaptações Realizadas | Forma de Verificação |
| :--- | :--- | :--- | :--- | :--- |
| *A preencher* | *A preencher* | *A preencher* | *A preencher* | *A preencher* |

---

## 11. Reflexão Final

*(Principal aprendizado, maior dificuldade e aspecto a aprimorar — a ser preenchido na consolidação final).*
